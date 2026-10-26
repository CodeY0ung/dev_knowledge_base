# HTTP 요청과 Servlet

```text
Client -- HTTP Request --> Server
Client <-- HTTP Response -- Server
```

- Servlet: HTTP 요청을 처리하는 Java 컴포넌트. Spring Controller와 역할이 유사하다.
- JSP: 서버에서 동적 HTML을 생성한다.
- Tomcat: Servlet Container. HttpServlet을 상속하는 것은 Servlet API를 사용하기 때문이다.

| 구분 | GET | POST |
|---|---|---|
| Form | `method="get"` | `method="post"` |
| Servlet | `doGet()` | `doPost()` |
| 용도 | 주로 조회 | 주로 제출 |

링크 `<a>`는 GET 요청. Form 값은 input의 `name`으로 전달한다.

## HTML Form

```jsp
<form action="FormServlet2" method="get">
  이름: <input name="name">
  성별: <input name="gender">
  취미:
  <input type="checkbox" name="hobby" value="walk">걷기
  <input type="checkbox" name="hobby" value="lying">눕기
  <input type="checkbox" name="hobby" value="sit">앉기
  <input type="checkbox" name="hobby" value="run">달리기
  <button type="submit">전송</button>
</form>
```

## Servlet

```java
@WebServlet("/FormServlet2")
public class FormServlet2 extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {
    String name = request.getParameter("name");
    String gender = request.getParameter("gender");
    String[] hobby = request.getParameterValues("hobby");
    response.setContentType("text/plain; charset=UTF-8");
    PrintWriter out = response.getWriter();
    out.println("이름: " + name);
    out.println("성별: " + gender);
    out.println("취미: " + (hobby == null ? "없음" : String.join(", ", hobby)));
  }
}
```

- `getParameter()`: 한 값 조회
- `getParameterValues()`: 같은 이름의 복수 값 조회
- `getWriter()`: 응답 본문 작성
- 체크박스 미선택 시 배열은 `null`일 수 있다.
- Form action 상대 URL과 `/FormServlet2` 절대경로는 컨텍스트 경로에 주의한다.

![손글씨](assets/request%3C-%3Eresponse-handwritten.jpg)
