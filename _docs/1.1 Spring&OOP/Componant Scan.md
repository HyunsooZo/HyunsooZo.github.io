---
title: Componant Scan
category: Spring Framework
order: 4
---

### ComponentScan & AutoWired

<div class="content-box">
Spring은 설정정보가 없어도 자동으로 Spring Bean을 등록하는<br>
<span class="emphasis">@ComponentScan</span> 이라는 기능을 제공한다.<br>
<span class="emphasis">@Component</span> Annotation이 붙은 Class를 스캔하여 자동등록한다.<br>
또, 의존관계도 자동으로 주입하는 <span class="emphasis">@Autowired</span> 기능도 제공한다.<br>
Class 의 Constructor 위에 <span class="emphasis">@Autowired</span> Annotation을 붙여주면 된다.
</div>

**ComponantScan 생성**
```java
package hello.core;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;

@Configuration
@ComponentScan(
        excludeFilters =
        @ComponentScan.Filter(type= FilterType.ANNOTATION,classes = Configuration.class)
        //(테스트를 위해 기존에 만들어둔) 직접 Bean 등록한 Component class 제외했음.
)
public class AutoAppConfig {

}
```

**Componant & Autowired 설정**
```java
package hello.core.member;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component // @Component 어노테이션 삽입
public class MemberServiceImpl implements MemberService {
    private final MemberRepository memberRepository;

    @Autowired // 생성자에 @Autowired 어노테이션 삽입
    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }

    public MemberRepository getMemberRepository() {
        return memberRepository;
    }
}
```

**TEST 성공**
```java
package hello.core.scan;

import hello.core.AutoAppConfig;
import hello.core.member.MemberService;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class AutoAppConfigTest {
    @Test
    void basicScan(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class);

        MemberService memberService = ac.getBean(MemberService.class);
        Assertions.assertThat(memberService).isInstanceOf(MemberService.class);
    }
}
```

### 탐색위치/기본스캔대상

`basePackages = "/path/.packagename"(예시)`<br>
->탐색할 패키지의 시작위치를 지정한다. 이 패키지를 포함하여 하위 패키지를 모두 탐색한다.<br>
(설정하지 않으면 모든 자바코드를 스캔한다.)

`basePackageClasses = AutoAppConfig.class(예시)`<br>
->지정한 클래스의 패키지를 탐색 시작 위치로 지정한다. 
(설정하지 않으면 `@ComponentScan`이 붙은 설정정보클래스가 패키지의 탐색시작위치가 된다.)

`Spring Boot에서 제공하는 방식` -> 패키지 위치를 지정하지 않고, 설정정보 클래스의 위치를 프로젝트 최상단에 둔다. <br>
예시:
<div class="content-box">
프로젝트구조
<div class="content-box">
com.hello<br>
com.hello.service<br>
com.hello.repository
..
</div>
프로젝트 구조가 위와 같다면,<br>
com.hello가 프로젝트 시작루트이므로, 여기에 AppConfig같은 메인설정정보를 두고<br>
@ComponentScan 을 붙이고 basePackages는 생략. <br><br>
이렇게 하면 com.hello 를 포함한 하위패키지는 모두 자동으로 컴포넌트 스캔의 대상이 된다.<br>
</div>

**ComponentScan 기본 Scan 대상**

|기본스캔대상|설명|부가기능|
|--|--|--|
|@Component|ComponentScan에서 사용|-|
|@Controller|Spring MVC Controller에서 사용|Spring MVC Controller로 인식|
|@Service|Spring Business Logic에서 사용| - |
|@Repository|Spring 데이터 접근 계층에서 사용|Spring Data 접근 계층으로 인식하고<br>데이터 계층의 예외를 스프링 예외로 변환|
|@Configuration|Spring 설정정보에서 사용|스프링설정정보로 인식,<br>스프링 빈이 싱글톤 유지하도록 추가처리


***참고***

사실 Annotation에는 상속관계라는 것이 없다. <br>
그래서 Annotation이 특정 Annotation을 상속하는 것처럼 동작하는 것은 <br>
Java 언어가 지원하는 기능이 아니고 Spring에서 지원하는 기능이다.<br>