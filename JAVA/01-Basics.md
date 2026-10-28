# Java 기초 문법

- **8/24**: `print`, `println`, `printf`, 변수 선언·초기화, 정수 산술 연산.
- **8/25~8/26**: 조건문·반복문·배열·메서드를 사용하는 기초 문제 실습.

```java
int n = 3;
int m = 20;
System.out.println(n + m);
System.out.println(m % n);
```

8/26 연산 퀴즈는 `Random`, `Scanner`, switch 표현식을 사용한다. 배열 길이가 100일 때 `nextInt(100) + 1`을 배열 인덱스로 사용하면 100번 인덱스에서 오류가 날 수 있으므로 `nextInt(100)`이 적합하다.

## 핵심 동작

`System.out.print()`는 줄바꿈 없이, `println()`은 줄바꿈과 함께 출력한다. `printf("%d", n)`은 형식 지정 출력이다.

```java
int[] numbers = {1, 2, 3};
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
int mode = 1;
int value = switch (mode) {
    case 1 -> 10 + 5;
    case 2 -> 10 - 5;
    default -> 0;
};
```

- 배열 인덱스: 0부터 `length - 1`까지.
- `Scanner.nextInt()`: 표준 입력에서 정수 읽기.
- `Random.nextInt(bound)`: 0 이상 `bound` 미만의 정수 생성.
- `switch` 표현식은 값을 반환하며 블록을 사용하면 `yield`로 값을 반환한다.

**원본:** [d0824](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0824), [d0825](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0825), [d0826](https://github.com/CodeY0ung/BNK_bootcamp/tree/main/src/d0826)
