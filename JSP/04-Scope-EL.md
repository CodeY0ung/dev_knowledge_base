# JSP의 Scope, Attribute, EL

| 저장소 | 범위 | EL 접근 |
|---|---|---|
| pageContext | JSP 페이지 | `pageScope.x` |
| request | HTTP 요청 | `requestScope.x` |
| session | 사용자 세션 | `sessionScope.x` |
| application | 웹 애플리케이션 | `applicationScope.x` |

```jsp
<%
  pageContext.setAttribute("x", "page 영역 데이터");
  request.setAttribute("x", "request 영역 데이터");
  session.setAttribute("x", "session 영역 데이터");
  application.setAttribute("x", "application 영역 데이터");
%>
${x}
${requestScope.x}
```

- EL `${x}` 검색 순서: page → request → session → application
- JSP Java 지역 변수는 EL이 바로 읽지 못한다. Attribute 등록이 필요하다.
- Parameter는 클라이언트 입력: `request.getParameter("name")`
- Attribute는 서버 내부 저장: `setAttribute()`, `getAttribute()`
- session은 타임아웃 또는 무효화될 수 있으며 영구 저장소가 아니다.

## Servlet에서 Scope 사용

```java
request.setAttribute("x", "request-x");
HttpSession session = request.getSession();
session.setAttribute("x", "session-x");
ServletContext application = request.getServletContext();
application.setAttribute("x", "application-x");
request.getRequestDispatcher("test.jsp").forward(request, response);
```

Servlet은 request를 매개변수로 받고 session/application은 호출로 얻는다. 일반 Servlet에는 JSP pageContext가 제공되지 않는다. 위 Forward 후 test.jsp의 `${x}`는 page 영역에 x가 없다면 request-x를 출력한다.

## EL 연산자 실습 (d1027)

| 연산 | 기호 | 키워드 |
|---|---|---|
| 나머지 | `%` | `mod` |
| 같음 | `==` | `eq` |
| 다름 | `!=` | `ne` |
| 초과 / 미만 | `>` / `<` | `gt` / `lt` |
| 이상 / 이하 | `>=` / `<=` | `ge` / `le` |

```jsp
${10 mod 3}
${10 % 3}
${10 eq 10}
${10 ne 10}
${10 gt 9}
${8 lt 9}
${8 ge 7}
${7 le 8}
```

### 파라미터와 삼항 연산자

```jsp
${not empty param.id}
${param.id == null}
${param.id == ""}

<% pageContext.setAttribute("score", 50); %>
<p>${score >= 60 ? "합격" : "불합격"}</p>
```

- `param.id`는 URL 쿼리 문자열이나 폼으로 전달된 `id` 파라미터를 조회한다.
- `not empty`는 null 또는 빈 문자열일 때 false가 된다.
