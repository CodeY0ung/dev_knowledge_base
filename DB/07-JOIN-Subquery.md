# JOIN · 서브쿼리

**학습: 9/16~17**

| 구문 | 실습 목적 |
|---|---|
| `INNER JOIN ... ON` | 일치하는 행 연결 |
| `JOIN ... USING` | 동일한 이름의 조인 컬럼 사용 |
| `NATURAL JOIN` | 같은 이름의 컬럼을 기준으로 자동 조인 |
| `LEFT OUTER JOIN` | 왼쪽 테이블의 미일치 행도 포함 |
| Non-equi JOIN | `BETWEEN` 등 동등 비교 외의 조건 |
| Self JOIN | `emp`를 두 별칭으로 참조해 상급자 조회 |
| 서브쿼리 | 다른 SELECT 결과를 조건에 사용 |

```sql
SELECT s.stu_no, s.stu_name, e.sub_no
FROM student s
JOIN enrol e ON s.stu_no = e.stu_no;

SELECT a.empno, a.ename, b.empno AS manager_no, b.ename AS manager_name
FROM emp a JOIN emp b ON a.mgr = b.empno;

SELECT ename FROM emp
WHERE sal > (SELECT sal FROM emp WHERE ename = 'BLAKE');
```

- `NATURAL JOIN`은 의도하지 않은 동명이름 컬럼까지 조인될 수 있어 주의한다.
- **원본 교정:** Self Join 예제에서 상급자 번호를 `b.mgr`로 조회한 부분은 상급자 본인의 번호 `b.empno`로 조회하는 것이 질문에 맞다.
- **원본 교정:** `NEWYORK`은 예제 데이터 `NEW YORK`과 일치하지 않는다.

원본: [join.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0916/join.sql), [9/17 JOIN 실습](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0917/%EC%A0%9C%EB%AA%A9%20%EC%97%86%EC%9D%8C.sql)
