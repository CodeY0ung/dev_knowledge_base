# JDBC 활용 프로젝트

| 날짜 | 원본 | 학습 내용 |
|---|---|---|
| 9/14 | [d0914Library](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0914Library) | 도서관·도서·대여 관련 객체와 JDBC 초안 |
| 9/17 | [d0917](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0917) | 계좌 및 쿼리 실습 |
| 9/18 | [d0918](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0918) | 직원 정보 객체와 DAO |
| 9/21 | [d0921_reservation_program](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0921_reservation_program) | 회원·상품·예약 모델 및 Repository |

원본 프로젝트에서 구현이 완료되었는지 확인되지 않은 기능은 완료 기능으로 단정하지 않는다.

## Oracle SQL 스키마 원본

| 프로젝트 | SQL | 설계 테이블 |
|---|---|---|
| 도서관 (9/14) | [Library.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0914/Library.sql) | student, book, rental, onetimebooks |
| 직원 (9/18) | [00_employee.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0918/00_employee.sql) | employee |
| 제조 관리 (9/18) | [01_hamburger.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0918/01_hamburger.sql) | material, product, manufacturing, production_record 등 |
| 예약 (9/21) | [03_reservation.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0921/03_reservation.sql) | member, item, reservation 및 시퀀스 |

원본 SQL의 테이블 설계는 [05-DDL-Constraints-Modeling.md](05-DDL-Constraints-Modeling.md)에서 주제별로 정리했다.

## 테이블 관계를 코드로 연결하는 방법

```text
예약 등록
입력값 → Java 객체 → Repository/DAO → PreparedStatement → Oracle
조회 요청 → DAO → ResultSet → Java 객체 → 화면 또는 콘솔
```

- 도서관 설계: 학생, 책, 대여와 대여 대상 도서 사이의 관계를 외래키로 표현.
- 제조 관리: 자재·입고·제조·생산 기록·납품을 별도 테이블로 관리.
- 예약: 회원/상품/예약을 분리하고, 예약 상태를 제한된 값으로 관리.
- 직원 DAO: EMPLOYEE 컬럼을 `Employee` 객체의 ID·이름·급여·날짜 필드에 매핑.

이 프로젝트들은 개별 테이블과 참조 관계를 설계한 실습이며, 전체 서비스의 완성 여부를 뜻하지 않는다.
