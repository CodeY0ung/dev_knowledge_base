# AJAX로 ID 중복 확인

## 문제

회원가입 Form을 Submit하지 않고 버튼으로 ID를 Servlet에 전달해야 한다.

## JSP / JavaScript

```jsp
<form>
  ID: <input type="text" id="id">
  <button type="button" id="btn">중복 확인</button>
</form>
<div id="demo"></div>
<script>
document.querySelector("#btn").addEventListener("click", function () {
  const userId = document.querySelector("#id").value;
  const xhttp = new XMLHttpRequest();
  xhttp.open("GET", "SignUpFormServlet?id=" + encodeURIComponent(userId), true);
  xhttp.onload = function () {
    if (xhttp.status === 200) {
      document.querySelector("#demo").textContent = xhttp.responseText;
    }
  };
  xhttp.send();
});
</script>
```

## Servlet

```java
@WebServlet("/SignUpFormServlet")
public class SignUpFormServlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {
    String id = request.getParameter("id");
    response.setContentType("text/plain; charset=UTF-8");
    response.getWriter().print("aaa".equals(id) ? "중복된 id" : "사용 가능");
  }
}
```

- `XMLHttpRequest.open`의 URL에 Query String으로 `id`를 붙여 보낸다.
- `responseText`로 응답 내용을 받는다.
- 원본의 `open(..., false)`는 동기 방식이다. 비동기 `true`로 교정했다.
- `encodeURIComponent`로 사용자 입력을 URL 인코딩한다.
- 컨텍스트 경로에 따라 Servlet URL을 조정한다.
