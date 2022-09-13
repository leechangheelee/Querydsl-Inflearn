## **중급 문법**

![image](https://user-images.githubusercontent.com/79301439/189928823-715f8e5c-2762-4228-9ea0-01e576fc1c37.png)

```java
package study.querydsl.dto;

import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
public class MemberDto {

    private String username;
    private int age;

    public MemberDto(String username, int age) {
        this.username = username;
        this.age = age;
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189929041-9d67b1d3-5611-45fa-8faa-6297fefaa672.png)

```java
@Test
public void findDtoByJPQL() {
    List<MemberDto> result = em.createQuery("select new study.querydsl.dto.MemberDto(m.username, m.age) from Member m", MemberDto.class)
            .getResultList();

    for (MemberDto memberDto : result) {
        System.out.println("memberDto = " + memberDto);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189929293-d512f31b-8f4d-4577-aa22-46411248502a.png)

![image](https://user-images.githubusercontent.com/79301439/189929376-4d506936-897d-45cc-b63c-2578e0185603.png)

```java
@Test
public void findDtoBySetter() {
    List<MemberDto> result = queryFactory
            .select(Projections.bean(MemberDto.class,
                    member.username,
                    member.age))
            .from(member)
            .fetch();

    for (MemberDto memberDto : result) {
        System.out.println("memberDto = " + memberDto);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189929687-31b0993f-9d9f-43f4-ab6c-99649aeacc28.png)

```java
@Test
public void findDtoByField() {
    List<MemberDto> result = queryFactory
            .select(Projections.fields(MemberDto.class,
                    member.username,
                    member.age))
            .from(member)
            .fetch();

    for (MemberDto memberDto : result) {
        System.out.println("memberDto = " + memberDto);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189929973-0348aab1-e13c-4f62-b6e1-aebe303376f7.png)

```java
import lombok.Data;

@Data
public class UserDto {

    private String name;
    private int age;

    public UserDto() {
    }

    public UserDto(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

```java
@Test
public void findUserDtoByField() {
    List<UserDto> result = queryFactory
            .select(Projections.fields(UserDto.class,
                    member.username.as("name"),
                    member.age))
            .from(member)
            .fetch();

    for (UserDto userDto : result) {
        System.out.println("userDto = " + userDto);
    }
}

@Test
public void findUserDtoByField2() {
    QMember memberSub = new QMember("memberSub");
    List<UserDto> result = queryFactory
            .select(Projections.fields(UserDto.class,
                    member.username.as("name"),
                    ExpressionUtils.as(
                            JPAExpressions
                                    .select(memberSub.age.max())
                                    .from(memberSub), "age")
                    )
            )
            .from(member)
            .fetch();

    for (UserDto userDto : result) {
        System.out.println("userDto = " + userDto);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189930922-56eda624-4fa4-4155-85e4-577ee659e965.png)

![image](https://user-images.githubusercontent.com/79301439/189931125-81002850-de29-4ff8-86f5-5710e76c6695.png)

```java
@Test
public void findDtoByConstructor() {
    List<MemberDto> result = queryFactory
            .select(Projections.constructor(MemberDto.class,
                    member.username,
                    member.age))
            .from(member)
            .fetch();

    for (MemberDto memberDto : result) {
        System.out.println("memberDto = " + memberDto);
    }
}

@Test
public void findUserDtoByConstructor() {
    List<UserDto> result = queryFactory
            .select(Projections.constructor(UserDto.class,
                    member.username,
                    member.age))
            .from(member)
            .fetch();

    for (UserDto memberDto : result) {
        System.out.println("memberDto = " + memberDto);
    }
}
```
