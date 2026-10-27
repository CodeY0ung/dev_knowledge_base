# Oracle Trigger

**학습: 9/22**

`BEFORE`/`AFTER`와 `INSERT`/`DELETE` 트리거 실습. 변경 행의 기존 값은 `:OLD`, 신규 값은 `:NEW`로 접근한다.

```sql
CREATE OR REPLACE TRIGGER trg_del_emp
AFTER DELETE ON emp_copy
FOR EACH ROW
BEGIN
    INSERT INTO bq_emp_copy
    VALUES (:OLD.empno, :OLD.ename, :OLD.job, :OLD.mgr,
            :OLD.hiredate, :OLD.sal, :OLD.comm, :OLD.deptno,
            CURRENT_TIMESTAMP);
END;
/
```

- 삭제된 사원 데이터를 별도 백업 테이블에 기록.
- 새로 입력한 사원의 번호·이름·입력일을 `emp_insert_log`에 저장하는 INSERT 트리거도 실습.
- 데이터 무결성은 가능하면 제약조건으로 처리하고, 트리거는 자동 기록 등 실제 필요가 있을 때 사용한다.

원본: [01_trigger.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0922/01_trigger.sql)
