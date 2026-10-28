# Spring Controller 테스트와 통합 테스트

## Controller 테스트

`@WebMvcTest`는 MVC 계층을 중심으로 테스트하고, `MockMvc`는 실제 HTTP 서버 포트를 열지 않고 요청 처리를 검증한다. Service는 Mock으로 교체할 수 있다.

```java
@WebMvcTest(OperatorController.class)
class OperatorControllerTest {
    @Autowired MockMvc mockMvc;
    @MockitoBean OperatorService operatorService;

    @Test
    void create() throws Exception {
        mockMvc.perform(post("/api/operators/staff")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\":\"Kim\"}"))
            .andExpect(status().isCreated());

        verify(operatorService).createStaff(any(CreateOperatorRequest.class));
    }
}
```

- `post()`: 가짜 POST 요청 구성.
- `contentType()`: 요청 `Content-Type` 지정.
- `content()`: 요청 본문 지정.
- `perform()`: 요청 실행.
- `andExpect()`: 응답 상태 등 검증.
- `verify()`: Mock Service 메서드 호출 여부 검증.

위 테스트는 해당 요청을 수용하는 DTO·Controller와 필요한 설정이 있다는 전제의 학습 예시다.

## 통합 테스트

```java
@SpringBootTest
@AutoConfigureMockMvc
class OperatorIntegrationTest {
    @Autowired MockMvc mockMvc;
    @Autowired OperatorRepository operatorRepository;

    @Test
    void create() throws Exception {
        mockMvc.perform(post("/api/operators/staff")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\":\"Kim\"}"))
            .andExpect(status().isCreated());

        // 실제 DB에 저장된 결과를 Repository로 검증
    }
}
```

- `@SpringBootTest`: Spring 애플리케이션 컨텍스트를 넓은 범위로 구성.
- `@AutoConfigureMockMvc`: MockMvc 사용 설정.
- Mock 중심 Controller 테스트는 요청 매핑·응답·Service 호출을 검증한다.
- 통합 테스트는 Controller부터 데이터 접근까지 실제 결괏값을 검증한다.
- 저장 데이터 검증을 하는 경우 테스트 DB와 데이터 격리를 설정해야 한다.

## 참고

- [Notion REST - ControllerTest / IntegrationTest](https://app.notion.com/p/REST-3f540968d35f80b4bb24df194c5a8053)
