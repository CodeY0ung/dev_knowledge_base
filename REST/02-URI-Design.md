# REST API URI 설계

URI에는 보통 동작보다 **자원의 이름(명사)**을 사용하고, 작업은 HTTP 메서드로 구분한다.

| 작업 | 메서드와 URI |
|---|---|
| 목록 조회 | `GET /api/customers` |
| 상세 조회 | `GET /api/customers/15` |
| 생성 | `POST /api/customers` |
| 수정 | `PUT /api/customers/15` |
| 삭제 | `DELETE /api/customers/15` |

- 소문자·복수형 명사 사용을 권장한다. 의무적인 문법 규칙은 아니며 프로젝트 내 일관성이 중요하다.
- 자원 ID는 경로에 넣는다: `/api/customers/15`.
- 검색·필터는 쿼리 파라미터로 표현한다: `/api/customers?name=Kim`.
- 자원 관계는 `GET /api/customers/15/loans`처럼 나타낼 수 있다.
- 동일 정보를 `GET /api/loans?customerId=15`처럼 필터로 표현할 수도 있다.
- 작업 이력이 별도 자원이면 `POST /api/loans/100/extensions`처럼 생성 요청으로 모델링할 수 있다.

## Path Variable과 Query Parameter

```text
GET /api/customers/15
                   └─ 특정 고객 ID

GET /api/customers?name=Kim&page=0&size=20
                   └─ 목록 검색 및 페이징 조건
```

HTTP 메서드별 의미와 구체적인 URI를 함께 보아야 어떤 API인지 알 수 있다.

## 참고

- [Notion Controller - URL 작성 규칙](https://app.notion.com/p/3fc40968d35f8071878dc0fb9b4fc599)
