# Edge Function 전송 테스트 (d1101)

Supabase Edge Functions는 TypeScript/Deno로 작성한다. 채용 지원 알림을 보내는 HTTP 모듈은 입력 검증, Slack 전송 결과 분류, 오류 처리를 담당한다.

## HTTP 결과

| 응답 | 처리 상태 |
|---|---|
| 2xx | SENT |
| 429 | RETRY — Retry-After 확인 |
| 5xx | RETRY |
| 그 외 4xx | FAILED |
| 타임아웃·네트워크 오류 | RETRY — 전달 성공 여부 불확실 |

Slack으로 전송됐지만 응답 수신에 실패했다면 재시도 때 알림이 중복될 수 있다.

## Deno 테스트

- `deno check`: TypeScript 타입 검사.
- `Deno.test`: Deno 내장 테스트.
- fetch 함수를 인자로 주입하면 실제 HTTP 요청 없이 200, 429, 503, 타임아웃을 모의 테스트할 수 있다.
- 응답과 무관하게 메시지에 지원자 이름·이메일 등 개인정보가 포함되지 않는지 검증한다.

## Edge Function 처리 순서

```text
요청 인증 및 입력값 검증
→ DB에서 지원서 조회
→ 작업 선점 RPC
→ Slack 전송
→ 완료 처리 RPC (SENT / RETRY / FAILED)
```

학습 문서의 5단계는 PostgreSQL 연동을 다루며, 실제 자동 Webhook 연동이나 배포 완료까지 증명하는 자료는 아니다.

참고: [Notion 4단계](https://app.notion.com/p/40b40968d35f8033b15fe9a06aa6e6b5), [Notion 5단계](https://app.notion.com/p/40b40968d35f809e99f9cff67693046c)
