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

## d1028: Session의 role 활용

로그인 검증을 통과하면 서버에서 역할을 세션에 저장할 수 있다.

```java
HttpSession session = request.getSession();
session.setAttribute("role", role);
```

JSP에서는 `${sessionScope.role}`로 값을 출력할 수 있다.

```jsp
<script>
  const role = "${sessionScope.role}";
  // 화면 표시 제어 용도로만 사용
</script>
```

**주의:** JavaScript의 role 확인은 개발자 도구에서 우회 가능하다. 실제 접근 권한은 **Servlet/Filter 등 서버 측**에서 검사해야 한다. 현재 실습의 로그인 Servlet은 ID 존재 여부만 확인하고 비밀번호를 검증하지 않는다. 학습용 구현이며 실제 인증으로 사용하면 안 된다.

## d1029: 세션 정보 확인

```java
String sessionId = session.getId();
long created = session.getCreationTime();
long lastAccessed = session.getLastAccessedTime();
int timeoutSeconds = session.getMaxInactiveInterval();
session.setMaxInactiveInterval(60); // 60초
boolean isNew = session.isNew();

session.setAttribute("name", "James");
String name = (String) session.getAttribute("name");
Enumeration<String> names = session.getAttributeNames();
```

- `getCreationTime()`, `getLastAccessedTime()`은 밀리초 단위 시각이다. 두 값의 차이는 마지막 요청까지의 경과 시간이지 실제 화면 체류시간이 아니다.
- 세션 저장값은 `getAttribute()`로 읽고 `getAttributeNames()`로 저장된 이름을 순회한다.
- 서버는 보통 `JSESSIONID` 쿠키로 브라우저 요청과 서버 세션을 연결한다. 쿠키에 저장되는 것은 기본적으로 세션 ID이며 세션 Attribute 전체가 아니다.
- 세션 ID는 인증 후 갱신하는 등 세션 고정 공격에 대한 방어가 필요하다.
