## **기본 문법**

![image](https://user-images.githubusercontent.com/79301439/188438570-3c874d0d-fb9e-45e2-b870-693a1cbfc823.png)

```java
QMember qMember = new QMember("m"); //별칭 직접 지정
QMember qMember = QMember.member; //기본 인스턴스 사용
```

![image](https://user-images.githubusercontent.com/79301439/188438662-870a81a8-c484-4e58-bced-470a046b82f1.png)

```java
...
import static study.querydsl.entity.QMember.*;
...
public class QuerydslBasicTest {
    ...
    
    @Test
    public void startQuerydsl() {
        Member findMember = queryFactory
                .select(member)
                .from(member)
                .where(member.username.eq("member1")) //파라미터 바인딩 처리
                .fetchOne();

        assertThat(findMember.getUsername()).isEqualTo("member1");
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/188438879-9a5e240a-0ad8-407e-ad51-b4b3d88545cf.png)

```yml
spring:
  jpa:
    properties:
      hibernate:
        use_sql_comments: true
```

![image](https://user-images.githubusercontent.com/79301439/188438962-f7bdfb4a-a3d4-4720-8cf1-e95bac7a8418.png)
