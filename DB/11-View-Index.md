# View · Index · 실행 계획

**학습: 9/18, 9/21**

## View

```sql
CREATE VIEW c AS
SELECT * FROM emp JOIN dept USING (deptno);
SELECT * FROM c;
DROP VIEW c;
```

View는 SELECT 결과를 논리적 테이블 형태로 노출한다.

## Index

```sql
CREATE INDEX idx_stu_name ON student(stu_name ASC);
SELECT * FROM USER_INDEXES;
SELECT * FROM USER_IND_COLUMNS WHERE TABLE_NAME = 'STUDENT';
DROP INDEX idx_stu_name;
```

- Table Full Scan과 Index Scan 비교.
- `/*+ FULL(emp) */`, `/*+ INDEX(...) */` 힌트 실습.
- 실행 계획의 Operation, Cardinality, Cost 관찰.
- 인덱스가 항상 빠른 것은 아니다. 선택도, 테이블 크기, 변경 빈도 등을 고려한다.
- **원본 교정:** 데이터 딕셔너리의 기본 대문자 식별자 기준으로 `TABLE_NAME = 'STUDENT'`를 사용한다.

원본: [02_view.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0918/02_view.sql), [00_index.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0921/00_index.sql)
