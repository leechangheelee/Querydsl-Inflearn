## **스프링 데이터 JPA가 제공하는 Querydsl 기능**

![image](https://user-images.githubusercontent.com/79301439/191430456-4ef9af01-68b5-483e-b539-099e9520ad8c.png)

```java
public interface QuerydslPredicateExecutor<T> {

    Optional<T> findById(Predicate predicate);
    Iterable<T> findAll(Predicate predicate);
    long count(Predicate predicate);
    boolean exists(Predicate predicate);
    // … more functionality omitted.
}
```

![image](https://user-images.githubusercontent.com/79301439/191430555-29b91a3a-3fea-4740-802d-94adfb8bfabc.png)

```java
public interface MemberRepository extends JpaRepository<Member, Long>, MemberRepositoryCustom, QuerydslPredicateExecutor<Member> {
}
```

```java
Iterable<Member> result = memberRepository.findAll(
        member.age.between(10, 40)
                .and(member.username.eq("member1"))
);
```

![image](https://user-images.githubusercontent.com/79301439/191430848-a7fe53f1-74fe-4fee-a7b1-52f80f39f2e9.png)
