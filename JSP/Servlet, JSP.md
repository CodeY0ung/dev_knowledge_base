# Servlet, JSP

---

## Servlet

```jsx
spring의 controller 역할
요청을 받고 필요한 응답을 한다.

servlet은 html을 반환한다 라고 할 수 있다.
```

## Jsp

```jsx
html에 동적인 요소를 넣으려고 JSP가 탄생.
어떻게? JAVA로
<% %>
<%= %>
```

---

## Code

java servelet에서 사용하는 template임.

### src/main/webapp 하위에 jsp 파일 생성(템플릿)

```jsx
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>Index Page</h1>
	<hr>
	<a href = "p1.jsp">p1 page</a><br>
	<a href="/TestServlet">TestServlet 호출</a>
	
	<br>
	<form action="/FormServlet2" method="get">
		이름 : <input type="text" name="name"> <br>
		성별 : <input type="text" name="gender"> <br>
		취미(2개 이상 선택) <br>
		<input type="checkbox" name="hobby" value="walk">걷기
		<input type="checkbox" name="hobby" value="lying">눕기
		<input type="checkbox" name="hobby" value="sit">앉기
		<input type="checkbox" name="hobby" value="run">달리기 <br>
		<button type="submit">전송</button>
	</form>
</body>
</html>
```

### src/main/java/servelets 하위에 servlet 파일 생성 (서버)

```jsx
@WebServlet("/FormServlet2")
public class FormServlet2 extends HttpServlet {
	private static final long serialVersionUID = 1L;

	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		
		String name = request.getParameter("name");
		System.out.println("name: "+name);
		String gender = request.getParameter("gender");
		System.out.println("gender: "+gender);
		String[] hobby = request.getParameterValues("hobby");
		System.out.println("hobby1: "+hobby[0]);
		System.out.println("hobby2: "+hobby[1]);
		
		out.println("이름 :"+name);
		out.println("성별 :"+gender);
		out.println("취미1 :"+hobby[0]);
		out.print("취미2 :"+hobby[1]);
	}
}
```

HttpServlet 을 extend 할 수 있는 이유는 tomcat 때문임.

### jsp에서 java를 사용하는 방법.

```jsx
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%
// 서비스 메서드 안에 사용되는 코드. 함수 안에 사용되는 처리문.
	String s = "hello";
%>
	<h1><%= s %></h1> // 응답할 때
</body>
</html>

```

index 페이지는 하나만 있어야함. 기본 url이 될 페이지