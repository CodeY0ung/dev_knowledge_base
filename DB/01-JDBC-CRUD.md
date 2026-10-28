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

## d1028: 입력 후 COMMIT 확인

```sql
INSERT INTO practice_sample (code, title, quantity)
VALUES ('A001', 'sample', 10);
COMMIT;
```

- Oracle에서 트랜잭션을 확정해야 다른 세션에서 변경 결과를 확인할 수 있다(자동 커밋 설정에 따라 차이).
- JDBC 코드도 `autoCommit` 설정을 확인한다.
- 이 예시는 개념 복습용이며 실제 시험 데이터는 포함하지 않는다.

## JDBC 실행 순서

```java
String sql = "SELECT name FROM member WHERE id = ?";
try (Connection conn = DBUtil.getConnection();
     PreparedStatement ps = conn.prepareStatement(sql)) {
    ps.setInt(1, 1);
    try (ResultSet rs = ps.executeQuery()) {
        while (rs.next()) System.out.println(rs.getString("name"));
    }
}
```

- `Connection`: DB 연결·트랜잭션 제어.
- `PreparedStatement`: SQL 준비 및 `?`에 값 바인딩.
- `executeQuery()`: SELECT 후 `ResultSet` 반환.
- `executeUpdate()`: INSERT·UPDATE·DELETE 후 영향받은 행 수 반환.
- try-with-resources: JDBC 자원 자동 종료.
- JDBC 기본 auto-commit 설정에서는 각 SQL 문장이 자동 커밋될 수 있다. 명시적 트랜잭션에서는 `setAutoCommit(false)`, `commit()`, `rollback()`을 사용한다.
