# Oracle SQL 페이징

- **9/22**: `OrderDAO.findOrderList(int number)`에서 `OFFSET`과 `FETCH NEXT`로 주문 데이터를 페이지 단위 조회.

```sql
SELECT * FROM order_record
ORDER BY id ASC
OFFSET ? ROWS
FETCH NEXT ? ROWS ONLY
```

```java
pstmt.setInt(1, offset);
pstmt.setInt(2, pageSize);
```

- 조회 결과는 `List<Order>`로 변환한다.
- 한 페이지 조회 결과가 없거나 페이지 크기보다 작으면 반복을 중단한다.

**원본:** [OrderDAO.java](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0922/OrderDAO.java)

## Oracle SQL 실습 원본 (9/21~22)

```sql
SELECT * FROM emp ORDER BY empno ASC
OFFSET 3 ROWS FETCH NEXT 3 ROWS ONLY;
```

- `order_record` 테이블은 `GENERATED AS IDENTITY` 기본키와 `user_id`, `item_id`, `quantity` 컬럼으로 구성된다.
- 정렬 기준이 있어야 페이지 간 순서가 안정적이다. 유일한 컬럼을 정렬 조건에 사용하는 것이 좋다.

원본: [02_last.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0921/02_last.sql), [02_paging.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0922/02_paging.sql)
