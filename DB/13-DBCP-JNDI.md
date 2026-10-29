# Tomcat DBCP · DataSource · JNDI (d1029)

DBCP(Database Connection Pool)는 DB 연결을 매 요청마다 새로 만들지 않고 풀에서 빌려 재사용하여 연결 생성 비용을 줄이는 방식이다.

## Tomcat Resource 예시

```xml
<Context>
  <Resource name="jdbc/myoracle"
            auth="Container"
            type="javax.sql.DataSource"
            driverClassName="oracle.jdbc.OracleDriver"
            url="${DB_URL}"
            username="${DB_USER}"
            password="${DB_PASSWORD}"
            maxTotal="20"
            maxIdle="10"
            maxWaitMillis="-1" />
</Context>
```

- Tomcat의 Context Resource 설정을 이용한다.
- `maxTotal`: 풀이 관리하는 최대 연결 수.
- `maxIdle`: 유휴 연결 최대 수.
- `maxWaitMillis`: 풀이 고갈됐을 때 대기할 수 있는 시간 설정. `-1`은 무제한 대기.
- 위 `${DB_*}`는 **설명용 자리표시자**로 실제 적용 시 환경별 Tomcat 설정 방식에 맞춰 값을 넣어야 한다. 계정·비밀번호는 저장소에 공개하지 않는다.

## JNDI로 DataSource 얻기

```java
Context initial = new InitialContext();
Context env = (Context) initial.lookup("java:comp/env");
DataSource ds = (DataSource) env.lookup("jdbc/myoracle");
try (Connection conn = ds.getConnection()) {
    System.out.println("DB 연결 성공");
}
```

`java:comp/env`에서 애플리케이션 환경 리소스를 찾는다. `getConnection()`으로 빌린 연결을 `close()`하면 풀 사용 환경에서는 연결이 반환된다. 학습 중에는 JSP에서 호출했지만 일반적으로 DAO 등 데이터 접근 계층에서 관리한다.

> 실습은 개발용 Tomcat 인스턴스에 DB Resource를 설정하여 진행했다.
