# 회원 이미지 게시판 실습 (d1030)

Servlet + JSP + JDBC의 Model 2(MVC) 방식으로 이미지 게시판을 구성한 실습.

## 요구사항과 구현 현황

| 항목 | 학습 요구 | 확인된 구현 |
|---|---|---|
| 회원 | ID·비밀번호로 로그인 | 로그인·로그아웃 Servlet 존재 |
| 게시글 등록 | 설명·이미지·작성자·작성일 | 등록 및 업로드 구현 |
| 게시글 목록 | 회원·비회원 조회 | 목록 조회 구현 |
| 게시글 상세 | 항목 선택 시 상세 화면 | 상세 조회 구현 |
| 수정·삭제 | 본인 게시글만 수정·삭제 | 계획 항목, 구현 미확인 |
| 조회수 | 게시글 조회수 | 계획 항목, 구현 미확인 |
| 관리자 | 별도 관리자 기능 | 계획 항목, 구현 미확인 |

## 계층과 요청 흐름

```text
Browser → Servlet → Service → DAO → Oracle
             │
             ├─ request.setAttribute(...)
             └─ forward → JSP
```

| 계층 | 주요 책임 |
|---|---|
| Domain | Member, Board 데이터 |
| DTO | LoginRequest, RegistRequest, BoardResponse |
| Service | MemberService, BoardService |
| DAO | MemberDao, BoardDao |
| Servlet | Login, Logout, Regist, List, Detail |
| JSP | index, login, regist, list, detail |

### 게시글 등록

```text
GET /regist → 작성 폼
POST /regist
  → multipart 파일 수신
  → 파일 저장
  → RegistRequest 생성
  → BoardService.regist()
  → BoardDao.insert()
  → redirect /list
```

파일과 게시글 메타데이터를 분리하여 저장한다. 등록 후 Redirect하면 같은 POST를 새로고침으로 반복하는 문제를 줄일 수 있다.

### 목록과 상세 조회

```java
// 목록
request.setAttribute("list", boardService.findList());
request.getRequestDispatcher("list.jsp").forward(request, response);

// 상세
request.setAttribute("board", boardService.findByBoardId(boardId));
request.getRequestDispatcher("detail.jsp").forward(request, response);
```

- JDBC `PreparedStatement`로 목록 및 특정 게시글 조회.
- `BoardResponse`로 조회 결과를 JSP에 전달.
- 파일의 **서버 저장 경로**와 브라우저에서 요청하는 **이미지 URL**은 다르다. 이미지 URL로 접근할 수 있는 공개 경로 또는 별도의 다운로드 Servlet이 필요하다.

## 게시글 DB 설계

학습에서 분리한 테이블은 회원과 게시글이다.

```text
Member (member_id PK, user_id, password)
  1 ─── N
Board (board_id PK, member_id FK, description, filename, updated_at)
```

게시글의 `member_id`는 작성자를 참조한다. 원본 실습에서는 Oracle Sequence로 게시글 ID를 생성했고, 날짜는 `java.sql.Date.valueOf(LocalDate)`로 저장했다.

## 코드 확인 메모

- 실습 등록 Servlet은 `memberId` 쿠키의 값을 그대로 작성자 ID로 사용한다. **사용자가 쿠키를 변경할 수 있어 본인 인증·권한 확인에 사용할 수 없다.** 검증된 서버 측 세션을 통해 작성자 ID를 결정해야 한다.
- 작성자 입력을 `readonly`로 만드는 것도 위변조 방지는 아니다.
- 등록 Servlet의 GET 메서드는 쿠키를 순회하면서 쿠키 이름이 다를 때마다 홈으로 Forward하려 한다. 로그인 쿠키만 먼저 찾고, **반복문이 끝난 뒤 한 번만** 분기하는 편이 안전하다.
- JSP는 현재 공개 웹 경로에 있으므로 URL 직접 접근을 막으려면 `WEB-INF`로 옮기고 Servlet에서 Forward해야 한다.
- DAO에서 Connection, PreparedStatement, ResultSet을 닫는 코드가 없다. try-with-resources를 적용해야 한다.
- `getRealPath("/uploads")`에 저장한 파일 경로는 배포 위치에 종속되고, 원본 이름으로 저장하면 덮어쓰기 위험이 있다.
- 원본 `BoardDao`에는 `insert`, `findAll`, `findById`가 확인되며 `update`, `delete`는 아직 없다.

**참고:** [jsp_board 실습 저장소](https://github.com/CodeY0ung/jsp_board) (문서 이해에 필수 아님).
