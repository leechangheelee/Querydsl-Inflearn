## **중급 문법**

![image](https://user-images.githubusercontent.com/79301439/190889885-520d17ae-1948-4de1-b5d4-c72901cf5ebd.png)

```java
@Test
public void sqlFunction() {

    List<String> result = queryFactory
            .select(
                    Expressions.stringTemplate(
                            "function('replace', {0}, {1}, {2})",
                            member.username, "member", "M"))
            .from(member)
            .fetch();

    for (String s : result) {
        System.out.println("s = " + s);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/190889904-bc1a8746-1eae-478c-b8c8-09ac7fe6b6c3.png)

```java
@Test
public void sqlFunction2() {
    List<String> result = queryFactory
            .select(member.username)
            .from(member)
            .where(member.username.eq(
                    Expressions.stringTemplate("function('lower', {0})", member.username)))
            .fetch();

    for (String s : result) {
        System.out.println("s = " + s);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/190889941-0e5af25c-3d6a-4610-b8e6-8ed3bb314cd2.png)

```java
.where(member.username.eq(member.username.lower()))
```
