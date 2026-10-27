# PreparedStatement · DAO · 객체 매핑

- **9/14**: 도서관 프로젝트에서 JDBC 관련 쿼리 클래스 생성. `Querys.saveBook()`은 SQL이 빈 문자열인 미완성 실습.
- **9/17**: 계좌 및 쿼리 예제.
- **9/18**: `EmployeeDAO`를 통한 DB INSERT/SELECT, `PreparedStatement`, try-with-resources.

```java
String sql = "SELECT * FROM employee WHERE emp_id = ?";
try (Connection conn = DBUtil.getConnection();
     PreparedStatement pstmt = conn.prepareStatement(sql)) {
    pstmt.setInt(1, empId);
    try (ResultSet rs = pstmt.executeQuery()) {
        if (rs.next()) {
            // ResultSet 값을 Employee 필드에 매핑
        }
    }
}
```

- `executeQuery()`는 SELECT 결과인 `ResultSet`을 반환한다.
- `executeUpdate()`는 변경된 행 수를 반환한다.
- JDBC의 DAO와 JPA의 Repository는 모두 데이터 접근 역할을 담당할 수 있으며 명칭이 프레임워크별로 엄격히 고정되는 것은 아니다.

**원본:** [d0914Library](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0914Library), [d0917](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0917), [EmployeeDAO.java](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0918/EmployeeDAO.java)
