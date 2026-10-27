# 회원·매출 관리 프로그램 (d1027, 3~8교시)

- 소스: [CodeY0ung/exam_jsp](https://github.com/CodeY0ung/exam_jsp)
- 기술: Java Servlet, JSP, JDBC, Oracle DB
- 구현: 회원 등록 · 목록 조회 · 상세 조회/수정 · 회원별 매출 조회

## 구성

```text
Browser → Servlet → MemberService → MemberDao → Oracle DB
           ↓ request.setAttribute()
          JSP (forward) → HTML
```

| 역할 | 실제 파일 |
|---|---|
| 도메인 | `domain/Member.java` |
| 입력 DTO | `dto/UpdateMemberInfoRequest.java` |
| 매출 조회 DTO | `dto/MemberSaleResponse.java` |
| DB 연결 | `dao/DBUtil.java` |
| DB 접근 | `dao/MemberDao.java` |
| 비즈니스 호출 | `service/MemberService.java` |
| 요청 처리 | `servlet/*.java` |
| 화면 | `src/main/webapp/*.jsp` |

## 기능별 요청 흐름

| 기능 | Servlet 경로 | 처리 | JSP/이동 |
|---|---|---|---|
| 회원 등록 화면 | `/moveToRegister` | 다음 회원 번호 조회 | `register.jsp`로 forward |
| 회원 등록 | `/register` | 폼 입력 → `Member` → INSERT | `/`로 redirect |
| 회원 목록 | `/memberList` | `List<Member>` 조회 | `member_list.jsp`로 forward |
| 회원 상세 | `/memberInfo?id=...` | 회원 1명 조회 | `memberInfo.jsp`로 forward |
| 회원 수정 | `/updateMemberInfo` | 수정 DTO → UPDATE | `/`로 redirect |
| 매출 조회 | `/sales` | JOIN·SUM·GROUP BY | `salesList.jsp`로 forward |

## JOIN 결과와 DTO

회원 테이블과 매출 테이블의 집계 결과는 `Member`가 아닌 `MemberSaleResponse`에 매핑한다.

```sql
SELECT m.custno, m.custname, m.grade,
       SUM(mo.price) AS sales
FROM member_tbl_02 m
JOIN money_tbl_02 mo ON m.custno = mo.custno
GROUP BY m.custno, m.custname, m.grade
ORDER BY sales DESC
```

```java
// MemberDao.findMemberSaleList()
List<MemberSaleResponse> result = new ArrayList<>();
while (rs.next()) {
    MemberSaleResponse dto = new MemberSaleResponse();
    dto.setCustNo(rs.getInt("custno"));
    dto.setName(rs.getString("custname"));
    dto.setGrade(rs.getString("grade"));
    dto.setSales(rs.getInt("sales"));
    result.add(dto);
}
```

Servlet에서 `request.setAttribute("salesList", ...)`로 전달하고, `salesList.jsp`에서 해당 리스트를 출력한다.

## 실습 코드 확인 메모

- `member_list.jsp`, `salesList.jsp`는 JSTL 대신 Scriptlet의 `for`문으로 목록을 출력한다.
- `memberInfo.jsp` 입력 이름은 `city`인데, 수정 Servlet은 `city_code`를 읽으므로 값 전달이 일치하지 않는다.
- `MemberDao.findMemberSaleList()`에서 결과가 0행이면 `rs.next()`가 false이므로 DB에 JOIN 조건과 일치하는 데이터가 있는지 확인해야 한다.
- `getMaxCustNo() + 1`은 동시 등록 시 중복 가능성이 있어 실서비스에서는 시퀀스 등의 방식이 필요하다.
- DB 접속 정보가 `DBUtil.java`에 하드코딩되어 있다. 공개 저장소에는 비밀번호를 커밋하지 않는 것이 바람직하다.

> 실행 결과까지 확인한 것은 아니며, 위 내용은 저장소의 실제 소스코드를 기준으로 정리했다.
