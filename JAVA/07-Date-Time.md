# Java 날짜·시간 API

- **9/21**: `Instant`, `ZoneId`, 지역별 시간대, 날짜·시간 포맷팅 실습.
- 예약 프로그램에서 일시를 다루는 클래스와 연결.

## 날짜·시간 타입

| 타입 | 의미 |
|---|---|
| `LocalDate` | 날짜만 |
| `LocalDateTime` | 시간대 없는 날짜·시각 |
| `Instant` | UTC 기준 타임라인의 한 순간 |
| `ZoneId` | 지역 시간대 식별자 (예: `Asia/Seoul`) |
| `ZonedDateTime` | 지역 시간대를 포함한 날짜·시각 |

```java
Instant now = Instant.now();
ZoneId seoul = ZoneId.of("Asia/Seoul");
ZonedDateTime local = now.atZone(seoul);
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
System.out.println(local.format(formatter));
```

`Instant`는 사건 발생 시점을 저장하는 데, `ZonedDateTime`은 지역에 따른 날짜·시각을 표현하는 데 유용하다. 지역 시간대와 단순 UTC 오프셋은 같지 않으며 서머타임 규칙 등에서 차이가 발생할 수 있다.

**원본:** [d0921](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0921), [d0921_reservation_program](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0921_reservation_program)
