# Oracle JDBC 기본 CRUD

- **9/10**: `DriverManager`, `Connection`, `Statement`, `ResultSet`으로 조회·입력·수정·삭제.
- **9/11**: 전화번호부 프로그램에 CRUD 로직 분리 (`Insert`, `Select`, `Update`, `Delete`).

```java
Connection conn = DriverManager.getConnection(url, user, password);
Statement stmt = conn.createStatement();
int count = stmt.executeUpdate(sql); // INSERT/UPDATE/DELETE
```

문자열 연결로 SQL 값을 만드는 예제는 학습용이다. 실제 입력값에는 `PreparedStatement`와 `?` 바인딩을 사용한다. DB 접속정보는 문서에 옮기지 않는다.

**원본:** [d0910](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0910), [d0911](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0911)
