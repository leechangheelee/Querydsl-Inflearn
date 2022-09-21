## **실무 활용 - 스프링 데이터 JPA와 Querydsl**

![image](https://user-images.githubusercontent.com/79301439/191404166-4c7bacbe-402a-4d0d-89eb-fe86156f2899.png)

```java
package study.querydsl.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import study.querydsl.entity.Member;

import java.util.List;

public interface MemberRepository extends JpaRepository<Member, Long> {
    //select m from Member m where m.username = ?
    List<Member> findByUsername(String username);
}
```

![image](https://user-images.githubusercontent.com/79301439/191404261-94417e7c-c9f8-4722-8381-4297b832938b.png)

```java
package study.querydsl.repository;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;
import study.querydsl.entity.Member;

import javax.persistence.EntityManager;
import java.util.List;

import static org.assertj.core.api.Assertions.assertThat;

@SpringBootTest
@Transactional
class MemberRepositoryTest {

    @Autowired
    EntityManager em;

    @Autowired MemberRepository memberRepository;

    @Test
    public void basicTest() {
        Member member = new Member("member1", 10);
        memberRepository.save(member);

        Member findMember = memberRepository.findById(member.getId()).get();
        assertThat(findMember).isEqualTo(member);

        List<Member> result1 = memberRepository.findAll();
        assertThat(result1).containsExactly(member);

        List<Member> result2 = memberRepository.findByUsername("member1");
        assertThat(result2).containsExactly(member);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/191404359-a672286f-7a56-43c5-8aae-37c087b7d941.png)
