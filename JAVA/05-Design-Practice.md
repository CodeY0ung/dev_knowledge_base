# Java 객체 설계 실습

- **9/4**: 학생·교사 정보 관리 (`StudentRepository`, 관리 클래스).
- **9/7**: 직원 관리 (`EmployeeRepository`, `EmployeeService`), Map 기반 사전 실습.
- **9/8**: 콘솔 게시판의 사용자·회원·게시글·Repository·Service·Controller 구분.

```text
BulletinBoardController
 ├─ MemberService → MemberRepository
 └─ PostRepository
```

게시판은 로그인, 회원가입, 비회원 조회 및 게시글 탐색 흐름을 포함한다.

**원본:** [d0904](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0904), [d0907](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0907), [d0908BulletinBoard](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0908BulletinBoard)
