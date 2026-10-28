# 클래스와 객체지향

- **8/27**: 생성자, `private` 필드, Getter/Setter, 메서드로 잔액 변경 (`PigMoneyBox`).
- **8/28**: 클래스 사용과 `toString()` 등 객체 메서드 실습.
- **8/31**: 상속, 오버라이딩, `Animal[]` 다형성, 도형 추상화 및 인터페이스.
- **9/1**: TV/교통수단/마우스 예제로 인터페이스와 구현체 교체, `TvFactory`.

```java
Animal[] animals = {new Dog(), new Cat(), new Tiger()};
for (Animal animal : animals) {
    animal.cry();
}
```

상위 타입으로 서로 다른 하위 객체를 참조하고 각 객체의 오버라이딩 메서드를 호출한다.

## 캡슐화·생성자

```java
class MoneyBox {
    private int balance;
    MoneyBox(int initialBalance) { this.balance = initialBalance; }
    int getBalance() { return balance; }
    void deposit(int amount) {
        if (amount <= 0) return;
        balance += amount;
    }
}
```

필드를 `private`으로 숨기고 메서드로 변경을 제어한다. 생성자는 객체 생성 시 초기 상태를 설정한다.

## 상속과 다형성

```java
class Animal { void cry() { System.out.println("sound"); } }
class Dog extends Animal {
    @Override void cry() { System.out.println("bark"); }
}
Animal animal = new Dog();
animal.cry(); // bark
```

참조 변수 타입이 `Animal`이어도 오버라이딩된 메서드는 실제 객체(`Dog`)의 구현이 실행된다. 인터페이스 타입 또한 구현 객체를 바꿔 사용할 수 있으며, TV와 교통수단 예제에서 같은 목적을 연습했다.

**원본:** [d0827](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0827), [d0828](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0828), [d0831](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0831), [d0901](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0901)
