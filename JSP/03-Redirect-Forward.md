# Redirect와 Forward

```java
@WebServlet("/p2")
public class P2Servlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {
    response.sendRedirect("p2.jsp");
    // 또는
    // request.getRequestDispatcher("p2.jsp").forward(request, response);
  }
}
```

| 구분 | sendRedirect | forward |
|---|---|---|
| 처리 | 브라우저가 다시 요청 | 서버 내부에서 위임 |
| HTTP 요청 | 새 요청 | 기존 요청 |
| 주소창 | 변경 | 유지 |
| request Attribute | 유지되지 않음 | 유지됨 |
| session | 같은 세션이라면 유지 | 유지 |

```java
request.setAttribute("x", "Hello");
request.getRequestDispatcher("b.jsp").forward(request, response);
```

b.jsp에서 `${x}` 조회 가능. Forward는 Request를 복사하는 것이 아니라 동일 요청을 전달한다. Redirect는 새 요청이지만 쿼리 파라미터를 URL에 명시할 수 있다.

## 요청 전달 추가 필기 (d1027)

```java
request.getRequestDispatcher("/target").forward(request, response);
response.sendRedirect("target");
```

- `forward()`: 같은 요청·응답 객체로 서버 내부의 다른 Servlet/JSP 등에 처리를 위임한다.
- `sendRedirect()`: 브라우저에 이동할 주소를 전달하여 새 요청을 발생시킨다.
- Tomcat(Servlet Container)은 요청마다 `HttpServletRequest`, `HttpServletResponse`를 제공한다.
- **정정:** 모든 요청이 `DispatcherServlet`을 거치는 것은 아니다. `DispatcherServlet`은 Spring MVC에서 사용하는 프론트 컨트롤러다.
- **정정:** JSP도 URL로 직접 요청할 수 있다. 컨테이너가 JSP를 Servlet 소스로 변환·컴파일하여 실행한다.
- Servlet 하나가 반드시 페이지 하나에 대응하거나 다른 Servlet으로 전달하는 전담 Servlet이 필요한 것은 아니다.
