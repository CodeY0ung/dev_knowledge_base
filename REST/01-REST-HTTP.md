# REST와 HTTP

REST는 HTTP 기반 시스템에서 자원(Resource)을 URI로 식별하고, 자원의 표현(Representation)을 주고받는 아키텍처 스타일이다. Spring에 종속되지 않는다.

- URI는 자원을 식별한다: `/api/customers/15`
- HTTP 메서드는 요청 의미를 표현한다.
- JSON은 자원을 나타내는 표현 형식 중 하나이다.
- REST를 사용한다고 반드시 프론트엔드와 백엔드를 별도 프로젝트로 분리해야 하는 것은 아니다.

## HTTP 메서드

| 메서드 | 일반적인 용도 | 예시 |
|---|---|---|
| GET | 조회 | `GET /api/customers/15` |
| POST | 생성·처리 요청 | `POST /api/customers` |
| PUT | 자원 전체 교체 | `PUT /api/customers/15` |
| PATCH | 일부 변경 | `PATCH /api/customers/15` |
| DELETE | 삭제 | `DELETE /api/customers/15` |

## 상태코드

| 코드 | 의미 |
|---|---|
| 200 OK | 요청 성공, 필요한 경우 응답 본문 포함 |
| 201 Created | 자원 생성 성공 |
| 204 No Content | 성공, 응답 본문 없음 |
| 400 Bad Request | 잘못된 요청 |
| 404 Not Found | 자원을 찾지 못함 |
| 409 Conflict | 현재 자원 상태와 요청 충돌 |

`201`과 `204`는 서로 다른 의미다. 반환 타입에 `Void`를 사용해도 자동으로 `204`가 되는 것은 아니다.

## 요청과 응답

```http
POST /api/customers
Content-Type: application/json

{"name":"홍길동"}
```

```http
HTTP/1.1 201 Created
Content-Type: application/json

{"customerId":15,"name":"홍길동"}
```

`Content-Type`은 본문 데이터의 형식을 알린다. 위 예시는 생성된 데이터를 응답 본문으로 돌려주는 경우다.

## 참고

- [Notion REST 학습 페이지](https://app.notion.com/p/REST-3f540968d35f80b4bb24df194c5a8053)
