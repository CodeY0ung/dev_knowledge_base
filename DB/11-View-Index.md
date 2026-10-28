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

## 실행계획 확인 관점

| 개념 | 의미 |
|---|---|
| Full Table Scan | 테이블을 스캔해 조건 검색 |
| Index Scan | 인덱스 자료구조를 통해 탐색 |
| Cardinality | 옵티마이저가 예상한 결과 행 수 |
| Cost | 실행계획의 상대적 비용 추정 |

```sql
CREATE INDEX idx_emp_name ON emp(ename);
SELECT ename FROM emp WHERE ename = 'SMITH';
DROP INDEX idx_emp_name;
```

인덱스는 읽기 성능을 개선할 수 있지만 INSERT·UPDATE·DELETE 시 유지 비용이 추가된다. 실행계획의 사용 여부는 힌트뿐 아니라 옵티마이저의 판단에 따른다. View는 저장된 SELECT 정의를 이름으로 조회하는 논리적 객체다.
