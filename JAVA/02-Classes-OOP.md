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

**원본:** [d0827](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0827), [d0828](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0828), [d0831](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0831), [d0901](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0901)
