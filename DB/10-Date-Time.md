# Oracle 날짜 · 시간 · 시간대

**학습: 9/15~16, 9/21**

| 타입·함수 | 실습 내용 |
|---|---|
| `DATE` | 날짜와 초 단위 시각 |
| `TIMESTAMP` | 소수 초 단위까지 시각 |
| `TIMESTAMP WITH TIME ZONE` | 시간대 오프셋/지역 정보가 포함된 시각 |
| `SYSDATE`, `CURRENT_DATE` | DB 서버 시각 / 세션 시간대 기준 날짜 |
| `CURRENT_TIMESTAMP` | 세션 시간대 기준 현재 타임스탬프 |
| `TO_DATE`, `TO_CHAR` | 문자열 ↔ 날짜 변환·표시 |
| `ADD_MONTHS`, `MONTHS_BETWEEN`, `LAST_DAY`, `NEXT_DAY`, `TRUNC` | 날짜 계산 |

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD') FROM dual;
SELECT ADD_MONTHS(DATE '2026-01-25', 3) FROM dual;
CREATE TABLE server_time (
    id NUMBER PRIMARY KEY,
    current_time TIMESTAMP WITH TIME ZONE NOT NULL
);
```

Java의 `Instant`, `OffsetDateTime`, `ZonedDateTime`와 비교한 실습도 포함된다. DB 시간대 타입과 Java 타입의 실제 변환 결과는 JDBC 드라이버·설정에 따라 검증해야 한다.

원본: [00_test.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0915/00_test.sql), [01_time.sql](https://github.com/CodeY0ung/Oracle_practice/blob/main/d0921/01_time.sql)

## 날짜 연산

```sql
SELECT SYSDATE, CURRENT_TIMESTAMP FROM dual;
SELECT ADD_MONTHS(DATE '2026-01-25', 1) FROM dual;
SELECT TRUNC(SYSDATE) FROM dual;
SELECT TO_DATE('2026-09-15', 'YYYY-MM-DD') FROM dual;
```

- `DATE`에는 날짜와 시각(초까지)이 저장된다.
- `TIMESTAMP`는 소수 초를 표현한다.
- `TIMESTAMP WITH TIME ZONE`은 오프셋 또는 지역 시간대 정보와 함께 시각을 표현한다.
- `TO_DATE`는 문자열 파싱, `TO_CHAR`는 출력 형식 지정에 사용한다.
- Java `Instant`는 절대 시점, `OffsetDateTime`은 오프셋이 있는 날짜·시각이다.
