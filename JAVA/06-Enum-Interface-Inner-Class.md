# Enum · 인터페이스 · 내부 클래스

- **9/9**: Enum 상수·필드·생성자, 상태/등급 표현, 상수별 메서드 구현.
- **9/9**: `Printable`, `Scannable` 등 다중 인터페이스 구현 예제.
- **10/7**: 멤버 내부 클래스, 정적 중첩 클래스, 별도 클래스와 내부 클래스 비교.

```java
public enum Operation {
    PLUS {
        @Override public int calculate(int a, int b) { return a + b; }
    },
    MINUS {
        @Override public int calculate(int a, int b) { return a - b; }
    };
    public abstract int calculate(int a, int b);
}
```

**원본:** [d0909EnumTest](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0909EnumTest), [d0909PrinterTetst](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0909PrinterTetst), [d1007_inner_class](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d1007_inner_class)
