## **실무 활용 - 순수 JPA와 Querydsl**

![image](https://user-images.githubusercontent.com/79301439/191205373-d6750ab4-2cb1-428c-aa4e-cde9503eff41.png)

```java
...
@Repository
public class MemberJpaRepository {

    ...

    //회원명, 팀명, 나이(ageGoe, ageLoe)
    public List<MemberTeamDto> search(MemberSearchCondition condition) {
        return queryFactory
                .select(new QMemberTeamDto(
                        member.id,
                        member.username,
                        member.age,
                        team.id,
                        team.name.as("teamName")))
                .from(member)
                .leftJoin(member.team, team)
                .where(
                        usernameEq(condition.getUsername()),
                        teamNameEq(condition.getTeamName()),
                        ageGoe(condition.getAgeGoe()),
                        ageLoe(condition.getAgeLoe())
                )
                .fetch();
    }
    
    private BooleanExpression usernameEq(String username) {
        return hasText(username) ? member.username.eq(username) : null;
    }

    private BooleanExpression teamNameEq(String teamName) {
        return hasText(teamName) ? team.name.eq(teamName) : null;
    }

    private BooleanExpression ageGoe(Integer ageGoe) {
        return ageGoe != null? member.age.goe(ageGoe) : null;
    }

    private BooleanExpression ageLoe(Integer ageLoe) {
        return ageLoe != null? member.age.loe(ageLoe) : null;
    }

}
```

![image](https://user-images.githubusercontent.com/79301439/191205763-e5ba9f02-722d-4923-8222-5adc878adb19.png)

```java
//where 파라미터 방식은 이런식으로 재사용이 가능하다.
public List<Member> findMember(MemberSearchCondition condition) {
    return queryFactory
            .selectFrom(member)
            .leftJoin(member.team, team)
            .where(
                    usernameEq(condition.getUsername()),
                    teamNameEq(condition.getTeamName()),
                    ageGoe(condition.getAgeGoe()),
                    ageLoe(condition.getAgeLoe())
            )
            .fetch();
}
```
