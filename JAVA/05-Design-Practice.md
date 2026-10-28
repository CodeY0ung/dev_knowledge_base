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

## 역할별 책임

| 구성 요소 | 역할 |
|---|---|
| Model | 학생·직원·회원·게시글 데이터 표현 |
| Repository | 메모리에서 데이터 저장·조회 |
| Service | 회원가입·로그인 등 동작 연결 |
| Controller | 콘솔 입력을 받아 작업 실행 및 출력 |

```java
class MemberRepository {
    private final Map<String, Member> members = new HashMap<>();
    void save(Member member) { members.put(member.getId(), member); }
    Member findById(String id) { return members.get(id); }
}
```

위 코드는 학습 구조를 축약해 작성한 예제다. 실제 실습에서는 학생·교사 정보 관리, 직원 관리, 게시판에서 저장소 객체와 처리를 나누어 구현했다. 메모리 Repository는 프로그램 종료 시 데이터가 사라지며 JDBC 기반 DAO와 구분된다.

**원본:** [d0904](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0904), [d0907](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0907), [d0908BulletinBoard](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0908BulletinBoard)
