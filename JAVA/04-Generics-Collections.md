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

## 제네릭과 동적 배열

`<T>`는 저장할 타입을 사용 시점에 지정한다. 고정 크기 배열로 목록을 구현하면 공간이 가득 찼을 때 더 큰 배열을 만들고 기존 요소를 옮겨야 한다.

```java
class Box<T> {
    private T value;
    void set(T value) { this.value = value; }
    T get() { return value; }
}
Box<String> box = new Box<>();
box.set("Java");
```

## Collection 비교

| 타입 | 실습에서 확인할 특성 |
|---|---|
| `ArrayList` | 순서가 있고 인덱스로 접근 |
| `HashSet` | 중복 원소 저장하지 않음 |
| `HashMap` | 키를 통해 값 조회 |

```java
Set<String> names = new HashSet<>();
names.add("A");
names.add("A");
System.out.println(names.size()); // 1
```

동등한 객체를 HashSet에서 같은 값으로 취급하려면 `equals()`와 `hashCode()`가 일관되어야 한다.

## 와일드카드

`List<? extends Animal>`은 `Animal` 또는 하위 타입 리스트를 읽는 용도로 받는 데 유용하다. 이 리스트에는 일반적으로 새 `Animal` 객체를 추가할 수 없다.

**원본:** [d0903](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0903), [EqualsHashCodeTest.java](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0907/EqualsHashCodeTest.java), [d1007_generic_exteds_super](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d1007_generic_exteds_super)
