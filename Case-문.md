## **기본 문법**

![image](https://user-images.githubusercontent.com/79301439/189844078-91944f11-aa21-43e6-b39f-7089857924a3.png)

```java
@Test
public void basicCase() {
    List<String> result = queryFactory
            .select(member.age
                    .when(10).then("열살")
                    .when(20).then("스무살")
                    .otherwise("기타"))
            .from(member)
            .fetch();

    for (String s : result) {
        System.out.println("s = " + s);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189844252-63075757-1fc7-4bd2-8aa2-2766b3a2af7b.png)

```java
@Test
public void complexCase() {
    List<String> result = queryFactory
            .select(new CaseBuilder()
                    .when(member.age.between(0, 20)).then("0~20살")
                    .when(member.age.between(21, 30)).then("21~30살")
                    .otherwise("기타"))
            .from(member)
            .fetch();

    for (String s : result) {
        System.out.println("s = " + s);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189844491-09a23333-5960-4381-b60e-db3d714ba95e.png)

```java
@Test
public void rank() {
    NumberExpression<Integer> rankPath = new CaseBuilder()
            .when(member.age.between(0, 20)).then(2)
            .when(member.age.between(21, 30)).then(1)
            .otherwise(3);

    List<Tuple> result = queryFactory
            .select(member.username, member.age, rankPath)
            .from(member)
            .orderBy(rankPath.desc())
            .fetch();

    for (Tuple tuple : result) {
        String username = tuple.get(member.username);
        Integer age = tuple.get(member.age);
        Integer rank = tuple.get(rankPath);
        System.out.println("username = " + username + " age = " + age + " rank = " + rank);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189846280-0b503761-8591-4d38-b45a-a691de6bebb6.png)

![image](https://user-images.githubusercontent.com/79301439/189846320-af5b1b2e-18aa-4b17-9e74-a1ef4f0a1c79.png)

```
username = member4 age = 40 rank = 3
username = member1 age = 10 rank = 2
username = member2 age = 20 rank = 2
username = member3 age = 30 rank = 1
```
