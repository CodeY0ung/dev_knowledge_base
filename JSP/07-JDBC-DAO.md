# JDBC · DAO · Servlet → JSP

## 초기 JSP 실습: DB 직접 조회

```jsp
<%
  Class.forName("oracle.jdbc.OracleDriver");
  Connection conn = DriverManager.getConnection(url, user, password);
  Statement stmt = conn.createStatement();
  ResultSet rs = stmt.executeQuery("SELECT * FROM student");
  while (rs.next()) {
%>
<tr>
  <td><%= rs.getInt("stu_no") %></td>
  <td><%= rs.getString("stu_name") %></td>
  <td><%= rs.getString("stu_dept") %></td>
  <td><%= rs.getString("stu_gender") %></td>
</tr>
<% } %>
```

원본은 접속 URL, 사용자명 및 비밀번호를 직접 선언했지만 공개 문서에는 값을 제외했다.

## 계층 분리

```text
Browser → SelectServlet → StudentDao → Oracle DB
               ↑             ↓ List<Student>
               └─────────────┘
        request.setAttribute("sl", sl)
               ↓ forward
             select.jsp → HTML
```

```java
// StudentDao
public List<Student> getStudents() {
  String sql = "SELECT * FROM student";
  List<Student> students = new ArrayList<>();
  try (Connection conn = DbUtil.getConnection();
       PreparedStatement pstmt = conn.prepareStatement(sql);
       ResultSet rs = pstmt.executeQuery()) {
    while (rs.next()) {
      Student st = new Student();
      st.setStu_no(rs.getInt("stu_no"));
      st.setStu_name(rs.getString("stu_name"));
      st.setStu_dept(rs.getString("stu_dept"));
      st.setStu_height(rs.getInt("stu_height"));
      st.setStu_weight(rs.getInt("stu_weight"));
      st.setGender(rs.getString("stu_gender"));
      students.add(st);
    }
  } catch (SQLException e) {
    throw new RuntimeException("학생 조회 실패", e);
  }
  return students;
}
```

```java
@WebServlet("/select")
public class SelectServlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {
    StudentDao dao = new StudentDao();
    List<Student> sl = dao.getStudents();
    request.setAttribute("sl", sl);
    request.getRequestDispatcher("select.jsp").forward(request, response);
  }
}
```

- JSP EL로 `${sl}`을 조회할 수 있으므로 `pageContext.setAttribute("sl", sl)`를 다시 호출하지 않아도 된다.
- Driver JAR은 필요하지만 `Class.forName()` 호출은 JDBC 4 이후 드라이버 자동 등록 때문에 항상 필수는 아니다.
- try-with-resources로 JDBC 연결, Statement, ResultSet을 닫는다.
