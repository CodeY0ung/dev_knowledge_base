# Session을 이용한 로그인

```text
index.jsp → loginform.jsp → POST /login → LoginServlet
                                        ├ 성공: session.userId 저장 → success.jsp
                                        └ 실패: fail.jsp
GET /member → MemberServlet → Session 검사 → member.jsp
```

## loginform.jsp

```jsp
<form action="login" method="post">
  ID: <input type="text" name="user_id">
  PW: <input type="password" name="pw">
  <button type="submit">로그인</button>
</form>
```

## LoginServlet

```java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
  @Override
  protected void doPost(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {
    String savedId = "aaa";
    String userId = request.getParameter("user_id");
    if (savedId.equals(userId)) {
      request.getSession().setAttribute("userId", userId);
      response.sendRedirect("success.jsp");
    } else {
      response.sendRedirect("fail.jsp");
    }
  }
}
```

## MemberServlet

```java
@WebServlet("/member")
public class MemberServlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {
    HttpSession session = request.getSession(false);
    String userId = session == null ? null : (String) session.getAttribute("userId");
    if (!"aaa".equals(userId)) {
      response.sendRedirect("loginform.jsp");
      return;
    }
    request.getRequestDispatcher("/WEB-INF/views/member.jsp").forward(request, response);
  }
}
```

- 기존 form의 `/loginResult.jsp`, `name="id"`는 Servlet의 `/login`, `user_id`와 맞지 않아 수정했다.
- 공개 경로의 `member.jsp`는 직접 URL 접근을 차단하지 못한다. `WEB-INF`에 배치하고 Forward한다.
- 이 예제는 세션 흐름 학습용으로 **비밀번호 검증을 구현하지 않는다**. 실제 로그인은 안전한 인증 처리가 추가되어야 한다.
