# PostgreSQL 작업 큐 · Trigger · RPC (d1101)

## 알림 작업을 DB에 남기는 이유

외부 Slack API 호출을 지원서 저장 트랜잭션에 직접 묶기보다 **알림 작업을 먼저 영속화**하면 실패 원인과 재시도를 추적할 수 있다. 실습은 Transactional Outbox에 가까운 패턴을 다룬다.

```sql
create table applications (
  id uuid primary key default gen_random_uuid(),
  position text not null,
  created_at timestamptz not null default now()
);

create table notification_deliveries (
  id uuid primary key default gen_random_uuid(),
  application_id uuid not null references applications(id),
  event_type text not null,
  status text not null default 'PENDING'
    check (status in ('PENDING', 'PROCESSING', 'SENT', 'RETRY', 'FAILED')),
  attempt_count integer not null default 0,
  locked_until timestamptz,
  claim_token uuid,
  unique (application_id, event_type)
);
```

설명을 위한 최소화된 스키마이며 실제 명세에는 HTTP 상태·에러·재시도 예정 시각 등의 컬럼이 더 있다.

## INSERT Trigger와 멱등성

```sql
create or replace function enqueue_application_notification()
returns trigger language plpgsql as $$
begin
  insert into notification_deliveries(application_id, event_type)
  values (new.id, 'APPLICATION_CREATED')
  on conflict (application_id, event_type) do nothing;
  return new;
end;
$$;

create trigger trg_enqueue_application_notification
after insert on applications
for each row execute function enqueue_application_notification();
```

- Trigger는 지원서 INSERT와 **같은 DB 트랜잭션** 안에서 실행된다.
- `UNIQUE(application_id, event_type)`와 `ON CONFLICT DO NOTHING`으로 동일 이벤트의 작업 중복 생성을 막는다.
- Webhook이 중복 호출되어도 작업 수는 늘어나지 않는다. 별도 선점 조건으로 이미 완료된 작업을 다시 보내지 않도록 한다.

## 원자적 작업 선점

```sql
-- 작업 선점 RPC의 핵심 구조를 단순화한 예시
update notification_deliveries
set status = 'PROCESSING',
    attempt_count = attempt_count + 1,
    locked_until = now() + interval '60 seconds',
    claim_token = gen_random_uuid()
where id = :notification_id
  and status = 'PENDING'
returning *;
```

`SELECT`로 확인한 뒤 별도 `UPDATE`를 하면 경쟁 상태가 생길 수 있다. 조건부 `UPDATE ... WHERE ... RETURNING`을 하나의 문장으로 실행하면 동일 행을 여러 실행이 동시에 선점하기 어렵다. 실제 RPC는 도래한 `RETRY`와 잠금이 만료된 `PROCESSING`까지 고려한다.

## Fencing Token

오래된 처리 실행 A가 중단된 동안 잠금이 만료되어 B가 다시 선점했다고 가정한다. A가 나중에 종료되어 B의 상태를 덮어쓰지 않도록 **선점 때 받은 claim_token과 현재 token이 일치할 때만 완료 처리**한다.

```sql
update notification_deliveries
set status = 'SENT', locked_until = null
where id = :notification_id
  and status = 'PROCESSING'
  and claim_token = :claim_token
returning *;
```

Token 불일치 시 0행을 반환하므로 오래된 처리 실행의 DB 갱신을 막는다. 다만 이미 Slack으로 나간 요청까지 되돌리는 것은 아니다.

## 상태와 검증

```text
PENDING → PROCESSING → SENT
                     → RETRY → PROCESSING
                     → FAILED
PROCESSING (잠금 만료) → PROCESSING (재선점)
```

- 429: `Retry-After` 확인 후 재시도.
- 5xx·타임아웃: `RETRY` 처리, 시도 횟수 상한 필요.
- 일반적인 4xx 설정·입력 오류: `FAILED`.
- 동시성: 두 DB 세션이 동시에 claim해도 한 요청만 성공해야 함.
- 권한: `anon`·`authenticated`가 서버 전용 RPC를 실행할 수 없도록 제한.

**주의:** 실제 노트에는 `security invoker`를 이용한 RPC와 역할별 실행 권한 검증을 설명한다. RPC의 권한 제한만으로 DB 테이블의 직접 쓰기 권한까지 자동 차단되는 것은 아니므로 RLS·GRANT도 확인한다.

## 참고 자료

- [Notion 2단계: Trigger](https://app.notion.com/p/40b40968d35f80929b62d952c019783b)
- [Notion 3단계: RPC와 동시성 테스트](https://app.notion.com/p/40b40968d35f8087aa70c184ae466e74)
