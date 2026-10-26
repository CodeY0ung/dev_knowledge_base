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
