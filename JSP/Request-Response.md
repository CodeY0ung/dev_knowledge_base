# Servlet과 JSP의 요청·응답

## 1. 요청과 응답

```text
Client  ── HTTP Request ──>  Server
Client  <─ HTTP Response ──  Server
```

- **Request(요청)**: 클라이언트가 서버에 보내는 데이터
- **Response(응답)**: 서버가 클라이언트에 돌려주는 결과

## 2. GET과 POST

- **Form**: `method="get"` 또는 `method="post"`로 요청
- **링크(`<a>`)**: 클릭하면 GET 요청
- **입력 데이터**: `name`을 기준으로 서버에 전달하며, `request.getParameter("name")`으로 조회

## 3. Servlet에서 처리

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) {
    // GET 요청 처리
}

protected void doPost(HttpServletRequest request, HttpServletResponse response) {
    // POST 요청 처리
}
```

- `request`: 요청 객체 — `getParameter("name")`으로 값 읽기
- `response`: 응답 객체 — `getWriter()`로 응답 내용 작성
- Servlet은 요청을 처리하고 HTML 등을 응답할 수 있다.

## 4. JSP

- **JSP**: HTML 안에 서버 측 Java 코드를 작성할 수 있는 기술
- `<% ... %>`: Java 코드 작성
- `<%= ... %>`: 값을 HTML에 출력

> `<% %>`와 `<%= %>`는 JSP의 전통적인 문법이다.
