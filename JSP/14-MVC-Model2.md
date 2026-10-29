# JSP Model 1 / Model 2 (MVC) (d1029)

Model 1은 JSP에서 요청 처리와 화면을 함께 구성하는 방식이다. Model 2는 Servlet Controller와 JSP View의 역할을 분리한다.

```text
Browser → Servlet → Service → DAO → DB
             └─ request Attribute → forward → JSP
```

- Controller(Servlet): 요청·응답 및 화면 이동을 제어한다.
- Service: 비즈니스 로직을 처리한다.
- DAO: 데이터베이스에 접근한다.
- View(JSP): 결과를 출력한다.
- Service와 DAO 앞의 인터페이스는 선택적이다.
- JSP를 WEB-INF 아래에 두면 URL 직접 접근을 막고 Servlet을 통해 Forward할 수 있다.

## hidden input

```html
<input type="hidden" name="itemId" value="123">
```

hidden 필드는 화면에서 보이지 않지만 사용자가 수정할 수 있으므로 권한이나 데이터의 신뢰성을 보장하지 않는다.

## 서버 푸시

일반적인 요청·응답 외에 WebSocket, SSE 같은 방식으로 서버에서 업데이트를 전달할 수 있다.
