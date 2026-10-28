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
