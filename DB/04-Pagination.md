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
