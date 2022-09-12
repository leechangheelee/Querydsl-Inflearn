## **기본 문법**

![image](https://user-images.githubusercontent.com/79301439/189625291-30a437ab-7c4b-4770-9b73-4e0e3658e978.png)

![image](https://user-images.githubusercontent.com/79301439/189625340-4f6a5db2-ca25-45a3-81d2-5028a87a151c.png)

```java
/**
 * 예) 회원과 팀을 조인하면서, 팀 이름이 teamA인 팀만 조인, 회원은 모두 조회
 * JPQL: SELECT m, t FROM Member m LEFT JOIN m.team t on t.name = 'teamA'
 * SQL: SELECT m.*, t.* FROM Member m LEFT JOIN Team t ON m.TEAM_ID=t.id and 
t.name='teamA'
 */
@Test
public void join_on_filtering() {
    List<Tuple> result = queryFactory
            .select(member, team)
            .from(member)
            .leftJoin(member.team, team).on(team.name.eq("teamA"))
            .fetch();

    for (Tuple tuple : result) {
        System.out.println("tuple = " + tuple);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189625573-f7821203-5c51-4faa-9f8f-8eb9e73f4c86.png)

```
tuple = [Member(id=3, username=member1, age=10), Team(id=1, name=teamA)]
tuple = [Member(id=4, username=member2, age=20), Team(id=1, name=teamA)]
tuple = [Member(id=5, username=member3, age=30), null]
tuple = [Member(id=6, username=member4, age=40), null]
```

![image](https://user-images.githubusercontent.com/79301439/189625858-86bb83c1-7907-421d-99da-44cb92e60b06.png)

![image](https://user-images.githubusercontent.com/79301439/189625918-6674832f-7f76-4ed8-a48c-9e7bc5801454.png)

```java
/**
 * 2. 연관관계 없는 엔티티 외부 조인
 * 예) 회원의 이름과 팀의 이름이 같은 대상 외부 조인
 * JPQL: SELECT m, t FROM Member m LEFT JOIN Team t on m.username = t.name
 * SQL: SELECT m.*, t.* FROM Member m LEFT JOIN Team t ON m.username = t.name
 */
@Test
public void join_on_no_relation() {
    em.persist(new Member("teamA"));
    em.persist(new Member("teamB"));
    em.persist(new Member("teamC"));

    List<Tuple> result = queryFactory
            .select(member, team)
            .from(member)
            .leftJoin(team).on(member.username.eq(team.name))
            .fetch();

    for (Tuple tuple : result) {
        System.out.println("tuple = " + tuple);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/189626302-1a147b95-0a87-4505-a549-67b415effd69.png)

![image](https://user-images.githubusercontent.com/79301439/189626356-41099153-34c4-4c8f-ac4d-b77ed055f80d.png)

```
tuple = [Member(id=3, username=member1, age=10), null]
tuple = [Member(id=4, username=member2, age=20), null]
tuple = [Member(id=5, username=member3, age=30), null]
tuple = [Member(id=6, username=member4, age=40), null]
tuple = [Member(id=7, username=teamA, age=0), Team(id=1, name=teamA)]
tuple = [Member(id=8, username=teamB, age=0), Team(id=2, name=teamB)]
tuple = [Member(id=9, username=teamC, age=0), null]
```
