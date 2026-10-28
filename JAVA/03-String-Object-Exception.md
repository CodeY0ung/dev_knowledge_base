# String · Object · static · 예외

- **8/28**: `substring`, `trim`, `equals`, `isEmpty`, `isBlank`, `compareTo`, 문자열 ↔ 숫자 변환.
- **9/2**: `static` 필드/메서드, `Object` 관련 예제, 사용자 정의 날짜 클래스, `try-catch-finally`.

```java
"  Hello Java  ".trim();
"apple".compareTo("banana");
Integer.parseInt("123");

try {
    // 실행할 코드
} catch (ArithmeticException e) {
    e.printStackTrace();
} finally {
    System.out.println("종료");
}
```

## 문자열 주요 메서드

| 메서드 | 동작 |
|---|---|
| `substring(2, 6)` | 인덱스 2부터 5까지 추출 |
| `trim()` | 문자열 양쪽의 기본 공백 제거 |
| `equals()` | 문자열 내용 비교 |
| `isEmpty()` | 길이가 0인지 판별 |
| `isBlank()` | 빈 문자열 또는 공백만 있는지 판별 |
| `compareTo()` | 사전식 순서 비교 |
| `Integer.parseInt()` | 정수 문자열을 `int`로 변환 |

```java
String value = "  Hello Java  ";
System.out.println(value.trim());  // Hello Java
System.out.println("".isEmpty());   // true
System.out.println(" ".isBlank());  // true
```

## static과 예외 처리

`static` 멤버는 개별 인스턴스가 아니라 클래스에 속한다. `try`에서 예외가 발생하면 대응하는 `catch`로 이동하고, `finally`는 종료 시 자원 정리 등에 사용한다.

```java
try {
    int n = Integer.parseInt("abc");
} catch (NumberFormatException e) {
    System.out.println("숫자 형식이 아닙니다.");
} finally {
    System.out.println("처리 종료");
}
```

**원본:** [StringTest.java](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0828/StringTest.java), [d0902](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0902)
