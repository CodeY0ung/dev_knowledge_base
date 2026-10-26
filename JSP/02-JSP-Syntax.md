# JSP 기본 문법

JSP는 HTML을 동적으로 생성하는 Java 서버 템플릿으로 Servlet 클래스로 변환되어 실행된다.

| 구분 | 문법 | 변환 위치 |
|---|---|---|
| 선언문 | `<%! ... %>` | Servlet 멤버 |
| 스크립틀릿 | `<% ... %>` | `_jspService()` 내부 |
| 표현식 | `<%= ... %>` | `out.print()` |

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" %>
<%!
    int globalCount = 0;
    public int multiply(int a, int b) { return a * b; }
%>
<%
    String s = "hello";
    int sum = 0;
    for (int i = 1; i <= 10; i++) sum += i;
%>
<h1><%= s %></h1>
<p>합: <%= sum %></p>
<p>곱: <%= multiply(5, 4) %></p>
```

- 스크립틀릿은 일반 Java 문장·지역 변수·반복문을 작성한다.
- 선언문의 필드는 여러 요청 사이에서 공유될 수 있으므로 스레드 안전성에 주의한다.
- 표현식에는 끝 세미콜론을 붙이지 않는다.
- index.jsp는 보통 시작 페이지로 사용하지만 반드시 하나만 존재해야 하는 것은 아니다.
- 현대 JSP에서는 Java 코드와 화면을 분리하려고 EL/JSTL을 사용한다.
