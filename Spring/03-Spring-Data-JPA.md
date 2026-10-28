# Spring Data JPA Repository 메서드

Spring Data JPA는 **Entity의 Java 필드명**으로 Repository 메서드 이름을 해석하고 쿼리를 생성할 수 있다. DB 컬럼명을 직접 이어 쓰는 규칙이 아니다.

```java
public interface CustomerRepository extends JpaRepository<Customer, Long> {
    List<Customer> findByNameContaining(String keyword);
    boolean existsByEmail(String email);
    long countByStatus(Status status);
}
```

## 주요 키워드

| 키워드 | 예시 | 의미 |
|---|---|---|
| `And`, `Or` | `findByNameAndPhone` | 조건 결합 |
| `Containing` | `findByNameContaining` | 문자열 포함 |
| `StartingWith` | `findByNameStartingWith` | 접두 문자열 |
| `GreaterThan` | `findByAmountGreaterThan` | 초과 |
| `Between` | `findByDateBetween` | 범위 |
| `Before`, `After` | `findByDateBefore` | 날짜 비교 |
| `IsNull` | `findByDeletedAtIsNull` | NULL |
| `In` | `findByStatusIn` | 값 목록 |
| `OrderBy...Desc` | `findByNameOrderByIdDesc` | 내림차순 |
| `Top5`, `First` | `findTop5ByStatus` | 결과 수 제한 |

## 연관관계 필드

```java
// Loan.customer → Customer.customerId 경로
List<Loan> findAllByCustomer_CustomerId(Long customerId);
```

`_`로 중첩 필드 경로를 명시할 수 있다. 필드 구조가 명확하면 구분자 없이도 해석되지만 이름 충돌에 주의해야 한다.

## 반환 타입과 페이징

- `Optional<Customer>`: 없을 수 있는 단일 결과. 실제로 여러 행이면 단일 조회가 실패할 수 있다.
- `List<Loan>`: 여러 결과.
- `boolean`: 존재 여부.
- `long`: 결과 개수.
- `Page<Loan>`: 페이지 정보와 함께 조회.

```java
Page<Loan> findAllByCustomer_CustomerId(Long id, Pageable pageable);
```

복잡한 조건은 지나치게 긴 메서드명보다 `@Query`(JPQL) 또는 동적 쿼리 사용을 검토한다.

## 참고

- [Notion Repository 메서드 작성 규칙](https://app.notion.com/p/3fc40968d35f808693ffd5d29b334e7d)
