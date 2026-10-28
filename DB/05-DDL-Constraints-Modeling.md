# 테이블 설계 · DDL · 제약조건

**학습: 9/14~15, 9/18, 9/22**

| 구문 | 용도 |
|---|---|
| `CREATE TABLE` / `DROP TABLE` | 테이블 생성·삭제 |
| `ALTER TABLE` | 컬럼·제약조건 변경 |
| `PRIMARY KEY` | 행 식별·중복/NULL 제한 |
| `FOREIGN KEY` | 다른 테이블의 키 참조 |
| `NOT NULL`, `UNIQUE`, `CHECK` | 입력값 제약 |

```sql
CREATE TABLE student (
    stu_no CHAR(9) PRIMARY KEY,
    stu_name VARCHAR2(20),
    stu_class VARCHAR2(1) CHECK (stu_class IN ('A','B','C'))
);
CREATE TABLE enrol (
    stu_no CHAR(9) REFERENCES student(stu_no),
    sub_no VARCHAR2(3)
);
```

- 도서관: `student`, `book`, `rental`, `onetimebooks`로 대여 관계 설계.
- 학생·과목·수강: `student`, `subject`, `enrol` 테이블 연결.
- 햄버거 제조: 자재·입고·제품·제조·생산·계획·납품 테이블 설계.
- 예약: 회원·상품·예약 테이블과 외래키 및 상태 `CHECK`.

**원본 교정:** `subject.sub_dept CHECK(sub_dept IN ('A,B,C'))`는 문자열 한 개만 허용한다. 3개 값을 허용하려면 `IN ('A','B','C')`로 작성한다. `emp(mgr)` 참조는 일반적으로 `emp(empno)`를 참조해야 하며 원본의 `REFERENCES emp(mgr)`는 참조 대상의 고유 제약조건을 확인해야 한다.

원본: [Library.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0914/Library.sql), [01_student.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0915/01_student.sql), [02_emp.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0915/02_emp.sql), [01_hamburger.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0918/01_hamburger.sql), [03_reservation.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0921/03_reservation.sql)

## d1028: 컬럼 정의 복습

- `CHAR(n)`: 고정 길이 문자열(길이가 부족하면 공백 패딩).
- `VARCHAR2(n)`: 가변 길이 문자열.
- `NUMBER(p,s)`: 전체 유효 자릿수 `p`, 소수 자릿수 `s`.
- Oracle에서 문자열 길이의 바이트/문자 기준은 컬럼 선언과 설정에 따라 다를 수 있다.

```sql
CREATE TABLE practice_sample (
  code CHAR(4) PRIMARY KEY,
  title VARCHAR2(30),
  quantity NUMBER(5)
);
```

실습에서 테이블 생성, 입력 데이터 검증을 복습했다. 시험용 원본 테이블 및 데이터는 복제하지 않는다.
