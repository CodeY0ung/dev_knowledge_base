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

**원본:** [StringTest.java](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0828/StringTest.java), [d0902](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0902)
