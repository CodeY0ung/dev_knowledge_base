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

## d1029: AJAX와 Cookie를 사용한 팝업

- Cookie: 사용자가 팝업을 일정 시간 보지 않겠다는 선택을 기억.
- AJAX: 페이지 이동 없이 Servlet에 Cookie 생성을 요청.
- DOM: 응답 성공 후 팝업을 화면에서 숨김.

```javascript
async function hidePopup() {
    const response = await fetch("/hidePopup", { method: "POST" });
    if (response.ok) document.getElementById("popup").hidden = true;
}
```

```java
@WebServlet("/hidePopup")
public class HidePopupServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws IOException {
        Cookie cookie = new Cookie("hidePopup", "true");
        cookie.setMaxAge(10);
        cookie.setPath("/");
        response.addCookie(cookie);
        response.setStatus(HttpServletResponse.SC_NO_CONTENT);
    }
}
```

다음 요청에서 `request.getCookies()`로 `hidePopup`을 확인하고 출력 여부를 결정한다. 쿠키 저장은 응답 이후에 적용되므로 같은 요청에서 쿠키를 바로 읽을 수는 없다. 위 코드의 경로는 루트 컨텍스트 기준이며 배포 경로에 맞게 변경해야 한다.
