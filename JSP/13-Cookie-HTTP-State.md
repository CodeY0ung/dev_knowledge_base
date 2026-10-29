# HTTP Cookie와 Session (d1029)

HTTP는 기본적으로 무상태(Stateless)이므로 각각의 요청에 사용자 식별 정보가 자동으로 연결되지 않는다.

- **Cookie:** 브라우저가 보관하고 조건에 맞는 다음 요청에 실어 보내는 데이터.
- **Session:** 서버 측에 사용자별 데이터를 저장하고 세션 ID로 구분하는 방식.
- Servlet 컨테이너는 보통 브라우저의 `JSESSIONID` Cookie로 서버 세션을 찾는다. 세션 속성 전체가 Cookie에 전달되는 것은 아니다.

## 쿠키 생성·유효시간·삭제

```java
Cookie cookie = new Cookie("preference", "yes");
cookie.setMaxAge(60);        // 60초
cookie.setPath("/");         // 경로 예시
response.addCookie(cookie); // 응답의 Set-Cookie 헤더에 추가
```

```java
Cookie expired = new Cookie("preference", "");
expired.setPath("/");        // 생성 시와 같은 Path
expired.setMaxAge(0);
response.addCookie(expired);
```

쿠키는 **응답 헤더(Set-Cookie)**를 받은 브라우저가 저장한 뒤, 다음 요청의 **Cookie 헤더**에 포함한다. 같은 요청에서 `request.getCookies()`를 다시 읽어도 방금 응답에 설정한 쿠키가 나타나지 않는다.

- `setMaxAge()` 미지정: 일반적으로 브라우저 세션 종료 시 사라지는 세션 쿠키(브라우저 복원 동작은 다를 수 있음).
- 브라우저는 이름·도메인·경로 등의 조합으로 쿠키를 구분한다. 같은 키로 저장 시 교체되는 것은 Java의 싱글톤 패턴 때문이 아니다.
- 쿠키 값은 제한된 문자로 표현해야 한다. 한글 등을 바로 Cookie 생성자에 넣기보다 UTF-8 기반 인코딩을 거쳐 사용한다.
- 개발자 도구에서 쿠키를 확인할 수 있다.

## 쿠키와 인증

실습에서는 로그인 ID 쿠키를 만들고 삭제하는 흐름을 연습했다. **ID 쿠키 존재 여부만으로 로그인 여부를 판단하면 위조할 수 있으므로 안전한 인증이 아니다.**

로그아웃 시에는 브라우저 쿠키 만료와 별개로 서버 세션 무효화(`session.invalidate()`)도 필요할 수 있다.

서버를 여러 대 쓰면 세션 저장소 공유·고정 라우팅 같은 방식 또는 서명된 토큰 인증 방식을 검토한다. 토큰도 저장 위치·만료·검증 등 보안 설계가 필요하며, 쿠키와 토큰은 서로 대체 관계가 아니다. 쿠키에 토큰을 담을 수도 있다.
