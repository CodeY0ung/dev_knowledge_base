# Spring IoC · DI · Bean (d1103)

## IoC와 Bean

- **IoC(Inversion of Control, 제어의 역전):** 객체 생성·연결·생명주기 관리의 제어를 애플리케이션 코드가 직접 수행하는 대신 Spring 컨테이너에 위임하는 방식.
- **Bean:** Spring IoC 컨테이너가 생성·등록·관리하는 객체. 모든 일반 Java 객체가 Bean인 것은 아니다.
- **컴포넌트 등록:** `@Component`, `@Service`, `@Repository`, `@Controller` 등의 어노테이션을 사용한다. **어노테이션만 붙인다고 항상 등록되지는 않으며**, 컴포넌트 스캔 대상이어야 한다.
- 설정 클래스의 `@Bean` 메서드로도 Bean을 등록할 수 있다.

```java
@Service
public class OrderService {
    public void register() { /* 주문 처리 */ }
}
```

## 의존성과 DI

A가 기능을 수행하려고 B를 사용해야 한다면 A는 B에 **의존**한다. 의존성은 필드에만 나타나는 것이 아니라 생성자 매개변수, 메서드 호출 등에서도 드러난다.

**DI(Dependency Injection, 의존성 주입)**는 A가 직접 B를 생성하지 않고 외부(예: Spring 컨테이너)에서 B를 전달받는 방식이다.

```java
@Repository
class OrderDao { }

@Service
class OrderService {
    private final OrderDao orderDao;

    // 생성자 주입: 생성자가 하나라면 @Autowired 생략 가능
    public OrderService(OrderDao orderDao) {
        this.orderDao = orderDao;
    }
}
```

- **생성자 주입:** 객체를 생성할 때 필요한 의존성을 전달. 필수 의존성에 주로 사용.
- **Setter 주입:** 객체 생성 후 Setter로 의존성을 전달. 선택적 의존성에 사용 가능.
- **필드 주입:** 필드에 `@Autowired`를 지정하는 방식. 테스트와 명시성 측면에서 생성자 주입을 우선 고려.

> 필기 교정: "Autowired 생성자 주입 안 해도 됨"은 **단일 생성자에서는 `@Autowired`를 생략할 수 있다**는 뜻이지, 생성자나 의존성 주입 자체가 필요 없다는 뜻은 아니다.

## 정보 은닉과 캡슐화

- `private` 등 접근 제한자로 내부 구현 접근을 제한하는 것이 정보 은닉의 주요 방법이다.
- 캡슐화는 데이터와 관련 동작을 클래스로 묶고 내부 상태 변경을 제어한다.
- **DI와 캡슐화는 서로 다른 개념**이다.
