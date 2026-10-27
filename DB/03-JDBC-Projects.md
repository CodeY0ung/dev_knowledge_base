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
