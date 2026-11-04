# AgencyOps AX P1 — 채용 지원 Slack 알림 (d1101)

## 목표와 범위

지원서가 등록되면 알림 작업을 DB에 남기고 Slack 채널로 전달하는 자동화 실습. 실제 회사 데이터 대신 더미 데이터를 사용한다.

- **기술:** Supabase PostgreSQL, Database Webhook, Edge Function(TypeScript/Deno), Slack Incoming Webhook
- **핵심 요구:** 지원서 저장, 중복 이벤트 처리, 전송 상태 기록, 실패 재시도, 유실 복구, 비밀키 보호.
- **범위 밖:** 채용 프론트엔드·Vercel 배포·LLM·RAG·대시보드.

## 처리 흐름

```text
지원서 등록
  ↓ INSERT
applications (PostgreSQL)
  ├─ DB Trigger → notification_deliveries에 PENDING 작업 생성
  └─ Database Webhook → notify-application Edge Function
                            ↓ 작업 claim RPC
                          PROCESSING
                            ↓ Slack HTTP POST
                     SENT / RETRY / FAILED
예약 재처리 함수 → 누락된 PENDING, RETRY, 잠금 만료 작업 재시도
```

- **DB Trigger:** 지원서 INSERT와 알림 작업 INSERT를 같은 트랜잭션으로 처리. Slack 통신 자체는 수행하지 않는다.
- **Database Webhook:** Edge Function의 즉시 실행을 촉발. 누락될 수 있으므로 작업 테이블을 기준으로 복구한다.
- **알림 테이블:** 전송 시도 수, 마지막 HTTP 코드, 오류, 재시도 예정 시각 등 저장.
- **외부 전송과 DB 저장은 별도 작업:** Slack 전송 도중 타임아웃이 발생하면 전송 성공 여부를 알 수 없어 다시 보낼 때 중복 알림이 발생할 수 있다.

**중요:** DB의 동일 이벤트 작업을 1개로 제한하는 것과 Slack 메시지를 정확히 1번만 발송하는 것은 다르다. 이 실습은 외부 Slack 전송의 exactly-once 보장을 제공하지 않는다.

## 개발 환경

```bash
mkdir agencyops-ax && cd agencyops-ax
git init
npm init -y
npm install supabase --save-dev
npx supabase init
npx supabase start
```

- **Supabase:** PostgreSQL·인증·파일 저장·Edge Functions 등을 제공하는 BaaS.
- **npm:** 패키지 및 프로젝트 의존성 관리.
- **npx:** 로컬 패키지의 실행 파일을 우선 실행하는 도구.
- **Migration:** SQL 변경 내용을 버전별로 저장하여 DB에 재현 가능하게 적용.
- `npx supabase db reset`은 **로컬 DB 데이터를 지우고 Migration을 재적용**하므로 운영 DB에서 함부로 실행하지 않는다.

## 학습 단계

| 단계 | 학습 내용 |
|---|---|
| 1 | 로컬 Supabase, npm·npx, 환경 구성 |
| 2 | applications / notification_deliveries, INSERT Trigger, Migration |
| 3 | claim/finish RPC, 원자적 선점·잠금·claim_token |
| 4 | Slack 전송 모듈, Mock fetch를 이용한 단위 테스트 |
| 5 | Edge Function과 PostgreSQL 연계, 서비스 간 인증·입력 검증 |

단계별 Notion 페이지에는 예제 SQL과 실행 지침, 체크리스트가 포함된다. 체크리스트 표시만으로 현재 배포 상태나 전체 시스템의 실시간 가동을 확정하지 않는다.

## 개인정보와 인증 경계

- Slack 메시지는 **지원서 ID, 지원 직무, 접수 시각** 정도로 최소화하고 이름·이메일은 보내지 않는다.
- Edge Function은 Webhook 본문을 무조건 신뢰하지 않고 인증 및 UUID를 검증한 다음 서버 권한으로 DB를 다시 조회한다.
- Slack Webhook URL 및 서비스 비밀키는 환경 Secret으로 관리하고 코드나 로그에 기록하지 않는다.
- RLS가 켜진 테이블 및 DB RPC에 대한 역할별 접근 권한을 점검한다.

## 참고 자료

- [개발 명세서](https://app.notion.com/p/19c40968d35f83a288ba81a7174456ec)
- [1단계 개발 환경 구축](https://app.notion.com/p/40b40968d35f803abb3dff14158cf591)
- [2단계 알림 작업 생성](https://app.notion.com/p/40b40968d35f80929b62d952c019783b)
- [5단계 Edge Function 연계](https://app.notion.com/p/40b40968d35f809e99f9cff67693046c)
