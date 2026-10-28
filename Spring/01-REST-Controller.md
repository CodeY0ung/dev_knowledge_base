# Spring REST Controller

`@RestController`는 `@Controller`와 `@ResponseBody`의 조합으로, 메서드 결과를 HTTP 응답 본문으로 직렬화한다. 일반적으로 JSON API를 만들 때 사용한다.

## 요청 처리 어노테이션

| 어노테이션 | 역할 |
|---|---|
| `@RequestMapping` | 공통 경로·요청 매핑 |
| `@GetMapping` / `@PostMapping` | HTTP 메서드별 매핑 |
| `@RequestBody` | JSON 등 요청 본문 → Java 객체 |
| `@PathVariable` | URI 경로 변수 |
| `@RequestParam` | 쿼리 파라미터 |
| `@ModelAttribute` | 요청 파라미터들을 객체에 바인딩 |
| `@RequestHeader` | 요청 헤더 값 |
| `@CookieValue` | 쿠키 값 |
| `@Valid` | DTO에 지정한 Bean Validation 검사 실행 |

```java
@RestController
@RequestMapping("/api/customers")
public class CustomerController {
    private final CustomerService service;

    public CustomerController(CustomerService service) {
        this.service = service;
    }

    @GetMapping("/{id}")
    public ResponseEntity<CustomerResponse> find(@PathVariable Long id) {
        return ResponseEntity.ok(service.findById(id));
    }

    @GetMapping
    public ResponseEntity<List<CustomerResponse>> search(
            @RequestParam(required = false) String name) {
        return ResponseEntity.ok(service.search(name));
    }

    @PostMapping
    public ResponseEntity<Void> create(@Valid @RequestBody CreateCustomerRequest request) {
        service.create(request);
        return ResponseEntity.status(HttpStatus.CREATED).build();
    }
}
```

이 코드는 Controller의 역할을 보여주는 축약 예시이며 DTO/Service 구현은 생략했다.

## ResponseEntity

```java
return ResponseEntity.ok(response);                 // 200 + JSON 등 본문
return ResponseEntity.noContent().build();          // 204 + 본문 없음
return ResponseEntity.status(HttpStatus.CREATED)
                     .body(response);               // 201 + 본문
return ResponseEntity.status(HttpStatus.CREATED)
                     .build();                      // 201 + 본문 없음
```

**주의:** `build()`는 응답 본문을 넣지 않는다. 데이터를 보내려면 `body(response)`를 사용한다.

## DTO 유효성 검사

```java
public record CreateCustomerRequest(
    @NotBlank String name,
    @NotBlank String phone
) {}
```

`@NotBlank`는 문자열이 null·빈 문자열·공백만 있는 문자열인지 검증한다. Controller에서 `@Valid @RequestBody`와 함께 사용할 수 있다.

## 참고

- [Notion Controller 어노테이션](https://app.notion.com/p/3f740968d35f8011a425efc110857566)
- [Notion ResponseEntity](https://app.notion.com/p/3fc40968d35f8081a812dab248d36a2c)
