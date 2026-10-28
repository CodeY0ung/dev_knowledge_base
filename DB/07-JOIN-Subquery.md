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

## JOIN 결과 비교

```sql
-- 일치하는 직원만
SELECT e.ename, d.dname FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno;

-- 부서에 속한 직원이 없어도 부서는 표시
SELECT d.dname, e.ename FROM dept d
LEFT JOIN emp e ON d.deptno = e.deptno;

-- 직원의 상급자 찾기
SELECT e.ename, manager.ename AS manager_name
FROM emp e LEFT JOIN emp manager ON e.mgr = manager.empno;
```

`INNER JOIN`은 일치 행만, `LEFT JOIN`은 왼쪽 테이블의 모든 행을 포함한다. Self Join은 같은 테이블에 서로 다른 별칭을 부여한다. `NATURAL JOIN`은 이름이 같은 모든 컬럼을 조인 조건으로 사용하므로 스키마 변경에 주의한다.
