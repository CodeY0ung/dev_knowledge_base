# 자료 대응표 및 교정 기록

| 원본 | 통합 문서 |
|---|---|
| GitHub `JSP/Request <-> Response.md` | 01, 02 |
| GitHub `JSP/Servlet, JSP.md` | 01, 02 |
| GitHub 루트 `Request-Response.md` | 01, 03, 04, 05 |
| Notion `JSP` | README |
| Notion `Request, Response` | 01, 02 |
| Notion `Servlet, JSP` | 01, 02 |
| Notion `d1026` | 03~07 |
| Notion `DB 연동` | 07 |
| Notion `trouble-shooting` 및 `01 JSP ← AJAX → Servlet` | 08 |
| 이전 대화 요청·응답, EL, JSTL, 로그인, DAO 실습 | 01~07 |

## 기술적 정정

| 기록 | 정리 |
|---|---|
| 로그인 Form과 Servlet 경로·입력 name 불일치 | 경로와 name 통일 |
| member.jsp를 Redirect하면 직접 접근 차단 | `WEB-INF`와 Forward 이용 |
| Session은 서버 종료까지 유지 | 타임아웃 또는 무효화 가능 |
| Forward는 request를 복사한다 | 같은 Request로 위임한다 |
| AJAX의 동기 `false` | 비동기 `true`로 수정 |
| 체크박스 `hobby[0]`, `hobby[1]` 고정 | null을 처리하고 배열 전체 사용 |
| JDBC 접속 정보 직접 기록 | 공개 문서에서는 값 제외 |
| JDBC `Class.forName` 호출은 필수 | 최신 드라이버 자동 등록 가능 |
| index.jsp는 오직 하나 존재 | welcome page 관례 |
| 필드명 `stu_grade`/`gender` 혼용 | 실제 getter 확인 필요 |

## 향후 작업 규칙

1. Notion의 날짜별 메모와 GitHub 최신 문서를 비교한다.
2. 기존 주제는 기존 문서를 수정하고 새 주제만 파일을 추가한다.
3. 자료 출처와 오류 교정 사항을 기록한다.
4. 원본 파일은 삭제하지 않는다.
5. Commit 형식: `d1027 <주제,내용>`.

이 문서는 2026-10-26까지 제공된 JSP 관련 자료에 한정한다.
