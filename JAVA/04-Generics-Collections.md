# 제네릭 · 컬렉션

- **9/3**: `ArrayList`, `Box<T>`, 제네릭 메서드, 가변 배열 구현.
- **9/7**: `Map`, `HashSet`, `equals()`·`hashCode()` 비교.
- **10/7**: `List<? extends Animal>` 상한 와일드카드와 제네릭 메서드 인자.

```java
public class GenericInfinityArray<T> {
    private Object[] arr = new Object[3];
    private int currentIdx;
    public void add(T value) {
        // 공간이 부족하면 배열 확장 후 값 추가
    }
}
```

`HashSet`에서 논리적 중복 판정은 `equals()`와 `hashCode()`의 구현에 영향을 받는다. 가변 배열 실습의 `get(int idx)`는 `idx == currentIdx`도 차단하도록 경계 조건을 점검해야 한다.

**원본:** [d0903](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0903), [EqualsHashCodeTest.java](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0907/EqualsHashCodeTest.java), [d1007_generic_exteds_super](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d1007_generic_exteds_super)
