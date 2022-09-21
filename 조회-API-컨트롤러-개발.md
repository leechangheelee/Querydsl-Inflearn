## **실무 활용 - 순수 JPA와 Querydsl**

![image](https://user-images.githubusercontent.com/79301439/191400091-eaa05d20-5ea5-4995-931e-45d3a64cbd09.png)

```yml
spring:
  profiles:
    active: local
```

![image](https://user-images.githubusercontent.com/79301439/191400203-f1f2fd08-1b9f-4717-b889-b142243101bc.png)

![image](https://user-images.githubusercontent.com/79301439/191400254-a82f3ef0-1fd6-4446-a009-b313c93f84ad.png)

```yml
spring:
  profiles:
    active: test
```

![image](https://user-images.githubusercontent.com/79301439/191400331-8559ac80-b80d-4d1d-b041-c5a0409f677e.png)

```java
package study.querydsl.controller;

import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Profile;
import org.springframework.stereotype.Component;
import org.springframework.transaction.annotation.Transactional;
import study.querydsl.entity.Member;
import study.querydsl.entity.Team;

import javax.annotation.PostConstruct;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

@Profile("local")
@Component
@RequiredArgsConstructor
public class InitMember {

    private final InitMemberService initMemberService;

    @PostConstruct
    public void init() {
        initMemberService.init();
    }

    @Component
    static class InitMemberService {
        @PersistenceContext
        private EntityManager em;

        @Transactional
        public void init() {
            Team teamA = new Team("teamA");
            Team teamB = new Team("teamB");
            em.persist(teamA);
            em.persist(teamB);

            for (int i = 0; i < 100; i++) {
                Team selectedTeam = i % 2 == 0 ? teamA : teamB;
                em.persist(new Member("member"+i, i, selectedTeam));
            }
        }
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/191400447-e6dd0b01-2708-4fe6-952d-fa535637b71b.png)

```java
package study.querydsl.controller;

import lombok.RequiredArgsConstructor;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import study.querydsl.dto.MemberSearchCondition;
import study.querydsl.dto.MemberTeamDto;
import study.querydsl.repository.MemberJpaRepository;

import java.util.List;

@RestController
@RequiredArgsConstructor
public class MemberController {

    private final MemberJpaRepository memberJpaRepository;

    @GetMapping("/v1/members")
    public List<MemberTeamDto> searchMemberV1(MemberSearchCondition condition) {
        return memberJpaRepository.search(condition);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/191400521-db180375-e519-4096-b56f-cd505c783cbc.png)

![image](https://user-images.githubusercontent.com/79301439/191400578-0060e208-1332-4006-93bb-e155fb08352b.png)
