# Spring Service · DTO

```text
Client → Controller → Service → Repository → DB
          요청/응답   비즈니스    데이터 접근
```

- **Controller:** 요청을 받아 DTO에 바인딩하고 Service 호출, HTTP 응답 생성.
- **Service:** 비즈니스 규칙 실행, 조회 결과를 응답 DTO로 변환.
- **Repository:** Entity 저장·조회.
- **Request DTO:** 클라이언트에서 받아야 할 값 정의.
- **Response DTO:** 클라이언트에 돌려줄 값 정의.

## Entity 목록을 DTO 목록으로 변환

```java
public List<CustomerInfoResponse> findAll() {
    return customerRepository.findAll()
            .stream()
            .map(CustomerInfoResponse::from)
            .toList();
}
```

| 메서드 | 의미 |
|---|---|
| `findAll()` | Entity 목록 조회 |
| `stream()` | Stream 처리 시작 |
| `map(CustomerInfoResponse::from)` | Entity 한 개를 DTO 한 개로 변환 |
| `toList()` | 결과를 List로 수집 |

DTO가 필요한 이유는 DB Entity 전체를 응답에 그대로 노출하는 대신 필요한 값만 전달하기 위해서다.

## 참고

- [Notion Service - stream](https://app.notion.com/p/3f740968d35f807cad44fb7777aa183b)
- [Notion DTO](https://app.notion.com/p/3fb40968d35f80468a87e6f17e19fafd)
