# JSP 종합 실습 기록 (d1028)

기존 JSP 연습 프로젝트를 초기화한 뒤 Servlet·JSP·JDBC 연동을 복습했다. 시험 문제 원문과 과제별 요구사항은 포함하지 않는다.

## 구현 요소

| 계층 | 학습 내용 |
|---|---|
| Domain | 데이터 모델 객체 |
| DTO | 등록 요청 / 목록·상태 조회 응답 객체 |
| Service | 요청에 따른 작업 연결 |
| DAO | JDBC를 통한 INSERT, SELECT |
| Servlet | 등록 및 목록 조회 요청 처리 |
| JSP | 입력 화면, 목록 화면, EL 출력 |

## 점검 항목

- 프로젝트 및 Tomcat 설정, JDBC 연결.
- DB 컬럼 타입 길이 설정과 INSERT 후 COMMIT.
- HTML `select`, CSS 배경색·가운데 정렬.
- JavaScript 필수값 검증, 등록 처리 후 알림 및 이동.
- 세션 role에 따른 화면 제어. 실제 접근 권한은 서버에서도 검사해야 함.

어려웠던 부분: 특별히 없었음.

관련 개념: [Session 역할](06-Session-Login.md) · [EL 포맷](04-Scope-EL.md) · [JDBC와 DAO](07-JDBC-DAO.md).
