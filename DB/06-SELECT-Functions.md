# SELECT · 조건 · Oracle 함수

**학습: 9/15~17, 9/21~22**

```sql
SELECT DISTINCT stu_dept FROM student;
SELECT stu_name, stu_dept
FROM student
WHERE stu_name LIKE '조%' AND stu_dept = '전기전자';
SELECT stu_no, sub_no, enr_grade + 10 AS plus10 FROM enrol;
```

| 구분 | 실습 문법 |
|---|---|
| 필터·정렬 | `WHERE`, `IN`, `BETWEEN`, `LIKE`, `ORDER BY` |
| 문자열 | `INITCAP`, `LOWER`, `UPPER`, `SUBSTR`, `INSTR`, `LENGTH`, `TRIM`, `REPLACE`, `LPAD`, `RPAD` |
| 숫자·날짜 | `POWER`, `ROUND`, `MONTHS_BETWEEN`, `TO_CHAR` |
| 집계 | `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `GROUP BY`, `HAVING` |

```sql
SELECT ename, INITCAP(ename), LENGTH(ename) FROM emp;
SELECT SUBSTR(ename, 1, 2), REPLACE(ename, 'E', 'Z') FROM emp;
```

- `WHERE`는 그룹화 전 행 조건, `HAVING`은 그룹화한 결과의 조건이다.
- 문자열·날짜 함수는 Oracle 전용 함수 또는 DBMS별 차이가 있을 수 있다.

원본: [01_student.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0915/01_student.sql), [01_textbook_training.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0916/01_textbook_training.sql), [제목 없음.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0917/%EC%A0%9C%EB%AA%A9%20%EC%97%86%EC%9D%8C.sql)
