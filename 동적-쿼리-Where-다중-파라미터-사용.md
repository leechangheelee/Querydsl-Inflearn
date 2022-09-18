## **중급 문법**

![image](https://user-images.githubusercontent.com/79301439/190888278-6dbf247c-3ed8-49ce-aa5a-5552e2c36e81.png)

```java
@Test
public void dynamicQuery_WhereParam() {
    String usernameParam = "member1";
    Integer ageParam = 10;

    List<Member> result = searchMember2(usernameParam, ageParam);
    assertThat(result.size()).isEqualTo(1);
}

private List<Member> searchMember2(String usernameCond, Integer ageCond) {
    return queryFactory
            .selectFrom(member)
            .where(usernameEq(usernameCond), ageEq(ageCond)) //where 내 null은 무시됨
            .fetch();
}

private BooleanExpression usernameEq(String usernameCond) {
    return usernameCond != null ? member.username.eq(usernameCond) : null;
}

private BooleanExpression ageEq(Integer ageCond) {
    return ageCond != null ? member.age.eq(ageCond) : null;
}
```

![image](https://user-images.githubusercontent.com/79301439/190888311-51be7032-d330-4060-899e-473dfb7ca07a.png)

![image](https://user-images.githubusercontent.com/79301439/190888316-2690480f-8de3-4dcf-9f5d-13fc3d1b0268.png)

```java
private BooleanExpression allEq(String usernameCond, Integer ageCond) {
    return usernameEq(usernameCond).and(ageEq(ageCond));
}
```

![image](https://user-images.githubusercontent.com/79301439/190888340-f97e8963-58c0-451f-9dbe-207b61fc37d7.png)
