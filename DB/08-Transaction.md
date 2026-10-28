# 트랜잭션 · COMMIT · ROLLBACK

**학습: 9/17, 9/22**

```sql
INSERT INTO test_tcl VALUES (4, 'hello', 'john');
SAVEPOINT a;
INSERT INTO test_tcl VALUES (5, 'world', 'yen');
ROLLBACK TO a;
COMMIT;
```

| 명령 | 의미 |
|---|---|
| `COMMIT` | 트랜잭션 변경 확정 |
| `ROLLBACK` | 미확정 변경 취소 |
| `SAVEPOINT a` | 트랜잭션 중간 지점 지정 |
| `ROLLBACK TO a` | 해당 저장 지점 이후 변경만 취소 |

- JDBC에서는 동일 Connection을 통해 트랜잭션을 제어하고, 필요시 `setAutoCommit(false)`를 사용한다.
- Oracle의 DDL은 일반적으로 실행 전후 암묵적 COMMIT이 발생한다. `TRUNCATE`도 DDL이다.
- 서로 다른 세션이 동일 행을 변경할 때 행 잠금으로 대기할 수 있다.

원본: [02tansaction.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0917/02tansaction.sql), [03_test.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0922/03_test.sql)

## JDBC에서 하나의 트랜잭션 처리

```java
try (Connection conn = DBUtil.getConnection()) {
    conn.setAutoCommit(false);
    try {
        // 같은 Connection으로 여러 SQL 실행
        conn.commit();
    } catch (SQLException e) {
        conn.rollback();
        throw e;
    }
}
```

- 여러 DML을 하나의 작업 단위로 묶을 때 같은 Connection을 사용한다.
- `ROLLBACK TO`는 Savepoint 이후 변경만 취소하지만 단순 `ROLLBACK`은 트랜잭션 전체의 미확정 변경을 취소한다.
- Oracle DDL에는 암묵적 커밋이 있어 DML과 동일한 방식으로 롤백되지 않는다.
