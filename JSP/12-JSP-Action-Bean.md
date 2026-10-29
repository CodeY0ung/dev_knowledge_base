# JSP 액션 태그와 JavaBean (d1029)

액션 태그는 JSP에서 서버 기능을 호출하는 XML 형태의 태그다.

## useBean · setProperty · getProperty

```jsp
<jsp:useBean id="member" class="domain.Member" scope="page" />
<jsp:setProperty name="member" property="name" value="철수" />
<jsp:setProperty name="member" property="age" value="30" />

이름: <jsp:getProperty name="member" property="name" />
나이: <jsp:getProperty name="member" property="age" />
이름(EL): ${member.name}
```

- `useBean`: 지정한 Scope에서 Bean을 조회하거나 새로 생성한다.
- `setProperty`: Setter를 호출해 속성 변경.
- `getProperty`: Getter를 호출해 값 출력.
- `id`는 Bean을 선언할 때, `name`은 사용할 때 해당 이름을 지정한다.
- `scope="page"`는 기본값이며 EL에서 해당 Bean에 접근할 수 있다.

## 폼 파라미터 자동 바인딩

```jsp
<jsp:useBean id="member" class="domain.Member" />
<jsp:setProperty name="member" property="*" />
```

요청 파라미터 이름과 JavaBean 속성 이름이 일치하면 변환 가능한 값을 Setter로 설정한다. JavaBean에는 접근 가능한 기본 생성자와 해당 Setter가 필요하다.

Spring MVC에서는 `@ModelAttribute`로 폼·쿼리 파라미터를 객체에 바인딩할 수 있다. `@RequestBody`는 주로 JSON 등 **요청 본문**을 객체로 읽는 기능이므로 같은 개념은 아니다. JSP 액션태그는 학습용으로 정리하며, 계층 분리 방식에서는 화면에 Java 비즈니스 처리를 넣지 않는다.
