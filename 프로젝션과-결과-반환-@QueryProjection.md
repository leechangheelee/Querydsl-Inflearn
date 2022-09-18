## **중급 문법**

![image](https://user-images.githubusercontent.com/79301439/190887273-553de19a-27f9-4b64-9120-be1d387dfb86.png)

```java
package study.querydsl.dto;

import com.querydsl.core.annotations.QueryProjection;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
public class MemberDto {

    private String username;
    private int age;

    @QueryProjection
    public MemberDto(String username, int age) {
        this.username = username;
        this.age = age;
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/190887297-3aa01e88-2cfa-486a-ad93-bf03734b0f90.png)

![image](https://user-images.githubusercontent.com/79301439/190887304-0fca59e9-0310-46b3-a133-699256496038.png)

```java
@Test
public void findDtoByQueryProjection() {
    List<MemberDto> result = queryFactory
            .select(new QMemberDto(member.username, member.age))
            .from(member)
            .fetch();

    for (MemberDto memberDto : result) {
        System.out.println("memberDto = " + memberDto);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/190887327-5da672d3-90f9-4d25-9cf7-41c91aad69cd.png)

![image](https://user-images.githubusercontent.com/79301439/190887337-98913acc-caba-44a4-8b19-0f89469e48d1.png)

```java
List<String> result = queryFactory
        .select(member.username).distinct()
        .from(member)
        .fetch();
```

![image](https://user-images.githubusercontent.com/79301439/190887363-ec3168ec-9931-42cf-b49d-cf19c11a84ea.png)
