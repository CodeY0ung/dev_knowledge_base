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

## Enum으로 상태 표현

```java
enum Status { REQUEST, CONFIRM, CANCELED }
Status current = Status.REQUEST;
```

임의의 문자열 대신 정해진 상수 집합을 타입으로 표현할 수 있다. 기존 `Operation` 예제처럼 Enum 상수마다 메서드 구현도 가능하다.

## 여러 인터페이스 구현

```java
interface Printable { void print(); }
interface Scannable { void scan(); }
class MultifunctionPrinter implements Printable, Scannable {
    public void print() { System.out.println("print"); }
    public void scan() { System.out.println("scan"); }
}
```

## 내부 클래스와 정적 중첩 클래스

- 멤버 내부 클래스: 바깥 클래스 인스턴스와 연결된다.
- `static` 중첩 클래스: 바깥 인스턴스 없이 생성할 수 있다.
- 학습 코드의 인쇄 기능과 내부 클래스 예제에서 선언 위치와 사용법을 비교했다.

**원본:** [d0909EnumTest](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0909EnumTest), [d0909PrinterTetst](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0909PrinterTetst), [d1007_inner_class](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d1007_inner_class)
