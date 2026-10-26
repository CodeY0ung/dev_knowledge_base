# JSTL로 학생 목록 출력

Scriptlet로 Java 반복문을 작성하면 HTML과 처리 코드가 섞인다.

```jsp
<% for (Student s : sl) { %>
<tr>
  <td><%= s.getStu_no() %></td>
  <td><%= s.getStu_name() %></td>
</tr>
<% } %>
```

JSTL의 `c:forEach`로 변경한다.

```jsp
<%@ taglib prefix="c" uri="jakarta.tags.core" %>
<c:forEach var="s" items="${sl}">
<tr>
  <td>${s.stu_no}</td>
  <td>${s.stu_name}</td>
</tr>
</c:forEach>
```

JSTL 라이브러리와 해당 버전의 태그 URI가 필요하다. 위 URI는 Jakarta Tags 3.x 기준이다.

## Servlet에서 목록 전달

```java
List<Student> sl = studentDao.getStudents();
request.setAttribute("sl", sl);
request.getRequestDispatcher("select.jsp").forward(request, response);
```

```jsp
<h1>학생 목록</h1>
<table>
  <tr><th>번호</th><th>이름</th><th>학과</th><th>성별</th><th>키</th><th>몸무게</th></tr>
  <c:forEach var="s" items="${sl}">
    <tr>
      <td>${s.stu_no}</td><td>${s.stu_name}</td>
      <td>${s.stu_dept}</td><td>${s.gender}</td>
      <td>${s.stu_height}</td><td>${s.stu_weight}</td>
    </tr>
  </c:forEach>
</table>
```

EL은 JavaBean getter를 사용한다. 원본 필기의 `stu_grade`/`gender` 등은 실제 Student 클래스의 getter와 대조해야 한다. DB 조회는 JSP가 아닌 DAO로 분리한다.
