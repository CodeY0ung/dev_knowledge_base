# Spring MVC Model · Command 객체 · 바인딩 (d1103)

## Spring MVC의 Model

MVC에서 **Model**은 View에 표시할 데이터·상태를 의미한다. Spring MVC의 `Model`은 Controller에서 View로 데이터를 전달하는 수단이다. MVC의 Model 전체가 `Model.addAttribute()` 객체 하나만을 뜻하지는 않는다.

```java
@Controller
class OrderController {
    @GetMapping("/orders")
    String list(Model model) {
        model.addAttribute("message", "주문 목록");
        return "orders/list"; // View 이름
    }
}
```

JSP 등 View에서는 `${message}`로 모델 속성을 읽을 수 있다.

## Command 객체와 데이터 바인딩

폼의 `name`과 객체의 JavaBean 속성명이 맞으면 Spring MVC가 입력값을 객체에 바인딩할 수 있다.

```html
<form action="/orders" method="post">
  <input name="customerName">
  <button type="submit">등록</button>
</form>
```

```java
public class OrderForm {
    private String customerName;
    public String getCustomerName() { return customerName; }
    public void setCustomerName(String customerName) {
        this.customerName = customerName;
    }
}

@PostMapping("/orders")
String create(@ModelAttribute OrderForm form) {
    // form.getCustomerName()으로 바인딩 값 확인
    return "redirect:/orders";
}
```

- `@ModelAttribute`: 폼 필드나 쿼리 파라미터를 객체의 속성에 바인딩.
- **바인딩**은 요청 데이터를 객체의 필드·속성에 연결하고 필요한 타입 변환을 수행하는 것. 값의 유효성을 검증하는 것과는 다르다.
- `BindingResult`: 바인딩 오류·검증 오류를 확인할 때 사용하며 관련 `@ModelAttribute` 인자 바로 뒤에 둔다.
- `@RequestBody`: JSON 같은 요청 본문을 객체로 역직렬화할 때 주로 사용한다.

## PathVariable · Redirect

```java
@GetMapping("/orders/{orderId}")
String detail(@PathVariable Long orderId, Model model) {
    model.addAttribute("orderId", orderId);
    return "orders/detail";
}

@PostMapping("/orders/save")
String save() {
    return "redirect:/orders"; // 브라우저에 새로운 요청 지시
}
```

- `@PathVariable`: 경로의 `{orderId}`를 매개변수로 받는다.
- `redirect:`: HTTP Redirect 응답으로 브라우저가 지정 주소를 다시 요청하게 한다.

## @ResponseBody

```java
@GetMapping("/orders/count")
@ResponseBody
int count() {
    return 3;
}
```

- `@Controller`의 메서드에 `@ResponseBody`를 붙이면 View 이름으로 해석하지 않고 반환값을 응답 본문으로 작성한다.
- JSP 기반 프로젝트에서도 AJAX 요청의 응답으로 JSON·텍스트를 돌려줄 수 있다.
- API 전용 프로젝트에서는 `@RestController`를 주로 사용하며 이는 `@Controller + @ResponseBody`와 같은 역할이다.
