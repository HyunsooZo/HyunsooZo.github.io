---
title: Dependency Auto Injection
category: Spring Framework
order: 6
---

### 다양한 의존관계 주입방법

**의존관계 주입은 크게 4가지가 있다.**

|DI Method|description|
|--|--|
|Constructor Injection|객체 생성 시점에 의존 객체를 전달하는 방식<br>생성자 호출시점에 딱 한번만 호출되는 것 보장<br> 불변,필수 의존관계에 사용<br>중요! 생성자가 딱 한개라면, @Autowired 생략가능!!<br><span class="emphasis">생성자 주입 방식이 주로 사용됨</span>|
|Setter Injection|Setter 메서드를 통해 의존 객체를 전달하는 방식<br>선택,변경 가능성이 있는 의존관계에 사용<br>Java Bean Property규약의 수정자 메서드 방식을 사용<br>|
|Feild Injection|feild를 통해 의존 객체를 전달하는 방식<br>외부에서 변경이 불가능하므로 테스트가 어려움<br>DI framework 없이는 아무것도 할 수 없다. <br> 자주 쓰이지 않음|
|Method Injection|일반 Method를 통해 의존 객체를 전달하는 방식<br>한번에 여러필드 주입받을 수 있음<br>자주 쓰이지 않음<br>|

**Constructor Injection(생성자주입) 사용이 권장되는 이유**

**불변**
<div class="content-box">
대부분의 DI는 한번일어나면 Application 종료시점까지 의존관계 변경할 필요가 없음.<br>
수정자 주입 사용시 , setXxx메서드를 public으로 열어두어야 함. <br>
생성자 주입은 객체 생성시 딱 1번만 호출되므로 불변하게 설계 가능
</div>

**누락**
<div class="content-box">
프레임워크 없이 순수 자바 코드를 단위테스트 할 때 수정자 주입 사용한 의존관계의 경우<br>
누락되는 Bean때문에 에러가 발생 할 수도 있다. 
</div>

**final keyword**
<div class="content-box">
생성자 주입을 사용하면 필드레 `final` 키워드 사용가능<br>
그래서 생성자에서 혹시라도 값이 설정되지 않는 오류를 컴파일 시점에서 막아줌.
**컴파일에러**는 정말 빠르고 좋은 오류임! 
</div>


### Option 처리

<div class="content-box">
주입할 Spring Bean 이 없어도 동작해야 할 때가 있다. <br>
그런데 <span class="emphasis">@Autowired</span>만 아용하면, <span class="emphasis">required</span> 옵션 기본값이 <span class="emphasis">true</span>로 설정되어있어 자동주입대상이 없으면 오류가 발생한다. 
</div>

**자동주입대상을 옵션으로 처리하는 방법은 다음과 같다**

`@Autowired(required = false)` : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출안됨.<br>
`org.springframework.lang.@Nullable` : 자동 주입할 대상이 없으면 Null 입력됨<br>
`Optional<>` : 자동 주입할 대상이 없으면 `Optional.empty`가 입력됨. 

**예시**
```java
    //아래 테스트코드에서 Member는 Spring Bean 이 아님

    @Autowired(required = false) // 호출 안됨. 
        public void setNoBean1(Member noBean1){
            System.out.println("noBean = " + noBean1);
        }

        @Autowired //호출 되나 Member 없을 시 null출력됨
        public void setNoBean2(@Nullable Member noBean2){
            System.out.println("noBean = " + noBean2);
        }


        @Autowired//호출 되나 Member 없을 시 Optional.empty 출력
        public void setNoBean3(Optional<Member> noBean3){
            System.out.println("noBean = " + noBean3);
        }

    //Output
    noBean = null
    noBean = Optional.empty
```


### Lombok(lib)과 최신트렌드

**Lombok?**
<div class="content-box">
Lombok은 Java Library 이다. <br> 
Lombok은 개발자가 반복적인 코드 작성을 줄여주고, 가독성을 향상시키는 데 도움이 되는 강력한 도구.<br>
Getter,Setter,Constructor,toString 등을 자동생성해준다. <br>
Annotation process를 사용하기 때문에 @Getter,@Setter,@ToString 등 추가필요<br>
IDE에 Lombok 플러그인을 설치필요, Lombok으로 생성된 코드를 직접 수정하지 않아야함 <br> 또한 Lombok에 대한 이해와 주의가 필요하며, 프로젝트 팀의 동의를 받아 사용해야 함.<br>
start.spring.io 에서 프로젝트 생성 시 Dependencies 에서 추가할 수 도 있다.
</div>

**Lombok 사용 예시**
```java

// @Getter ,@Setter, @ToString 사용예시
@Getter
@Setter
@ToString
public class HelloLombok {
    private String name;
    private int age;

    public static void main(String[] args){
        HelloLombok helloLombok = new HelloLombok();
        helloLombok.setName("hyunsoo");
        String name = helloLombok.getName();
        System.out.println("name = " + name);
        System.out.println(helloLombok.toString());
    }
// @RequiredArgsConstructor 사용예시
@Component
@RequiredArgsConstructor
public class OrderServiceImpl implements OrderService{

private final MemberRepository memberRepository;
private final DiscountPolicy discountPolicy;

}
```

**Lombok 수동으로 추가하기(build.gradle)**

**1.** 프로젝트의 build.gradle로 이동.

**2.** Lombok 추가 (Annotation process 사용)
```java
configurations {//build.gradle 내부에 추가
	compileOnly { extendsFrom annotationProcessor }
}

dependencies{ //dependencies 내부에 추가 
    ...
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'
	testCompileOnly 'org.projectlombok:lombok'
	testAnnotationProcessor 'org.projectlombok:lombok'
    ...
}
```
**3.** IntelliJ 우측의 gradle(코끼리) 클릭하여 refresh

**4.** 플러그인 설치 

- IntelliJ -> preferrence -> pulgIn -> Lombok 

**5.** Annotation process 켜기

- IntelliJ -> preferrence -> annotation process - > enable

**최신트렌드**
<div class="content-box">
최근에는 생성자를 딱 1개두고 `@Autowired`를 생략하는 방법을 주로 사용한다. <br>
여기에 Lombok Library의 `@RequiredArgsConstructor`를 사용하면 기능은 모두 제공되면서 코드는 깔끔하게 사용할 수 있다.
</div>


### 조회한 Bean이 두 개 이상일 때

<div class="content-box">
@Autowired는 Type으로 조회하기 때문에 Bean이 두 개 이상 조회될 수 있고 이로 인해 문제가 발생할 수 도 있다. <br>
<font color="red"> NoUniqueBeanDefinitionException: 
No qualifying bean of type 'hello.core.discount.DiscountPolicy' <br>
available: 
expected single matching bean but found 2: fixedDiscountPolicy,ratedDiscountPolicy</font>  <br>
</div>

**해결방법**

하위타입을 수정할 수도 있지만 이는 DIP를 위반하고 유연성이떨어지게 된다.<br>
그리고 이름만 다르고, 완전히 똑같은 타입의 SpringBean이 2개 인 경우에는 해결되지 않는다. <br>
Spring Bean을 수동등록하여 문제를 해결 할 수 도 있지만, 의존관계자동주입에서 해결하는 여러 방법이 존재한다. 

**해결방법(1)**

`@Autowired` **feild명 매칭 시키기**

`@Autowired`는 타입 매칭을 시도하고, 여러 빈이 존재 시 필드이름, 파라미터 이름으로 빈 이름을 추가 매칭한다. 

`@Autowired` 매칭 정리 : <br>
*1.* Type 매칭<br>
*2.* Type 매칭의 결과가 2개 이상일 때 feild명, parameter 명으로 Bean 이름 매칭

**[예시]**
```java
//기존코드
@Autowired
private DistcountPolicy discountPolicy

//필드명을 빈 이름으로 변경
@Autowired
private DistcountPolicy rateDiscountPolicy
```

**해결방법(2)**

`@Qualifier` -> `@Qualifier`끼리 매칭 -> Bean 이름 매칭 

`@Qualifier`는 추가구분자를 붙여주는 방식이다.(이름변경 x)<br>
주입 시 @Qualifier 를 붙여주고 동일한 이름을 적어주면 된다. 

`@Qualifier` 매칭 정리 : <br>
*1.* `@Qualifier`끼리 매칭 <br>
*2.* Bean 이름 매칭<br>
*3.* 매칭 실패 시 `NoSuchBeanDefinitionException`예외 발생

**[예시]**
```java
 @Autowired
    public OrderServiceImpl(MemberRepository memberRepository,@Qualifier("mainDiscountPolicy")DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

**해결방법(3)**

`@Primary` **사용하기**

`@Primary`는 우선순위를 정하는 방법이다.<br>
`@Autowired`시에 여러 번 매칭되면 `@Primary`가 우선권을 가진다. 

**[예시]**
```java
@Component
@Primary
public class RatedDiscountPolicy implements DiscountPolicy{...}
```

### Annotation 직접 만들기

`@Primary` 로 해결되지 않을 때 종종 Annotation을 만들어 쓰기도 한다. 

```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
@Qualifier("mainDiscountPolicy")
public @interface MainDiscountPolicy {
}
```

<div class="content-box">
Annotation에는 inheritance라는 개념이 없다. <br>
여러 Annotation을 모아서 사용하는 기능은 Spring이 지원해주는 기능이다.<br>
@Qualifier 뿐 아니라 다른 Annotation들도 함께 조합해서 사용할 수 있다.<br>
단적으로 @Autowired도 재정의 할 수 있다. 물론 Spring이 제공하는 기능을 뚜렷한 목적없이 무분별 하게 재정의하는 것은 피해야 한다. 
</div>

### 조회한 Bean이 모두 필요할때, List/Map

<div class="content-box">
의도적으로 해당타입의 Spring bean이 모두 필요한 경우도 있다.<br>
예를 들어 할인 서비스를 제공하는데, 클라이언트가 할인의 종류(rate,fix)를 선택할 수 있다고 가정해보자. Spring을 사용하면 소위말하는 <b>전략 패턴</b>을 매우 간단히 구현할 수 있다.
</div>

```java
public class AllBeanTest {

    @Test
    void findAllBean (){
        ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class,DiscountService.class);

        DiscountService discountService = ac.getBean(DiscountService.class);
        Member member = new Member(1L, "userA", Grade.VIP);
        int discountPrice = discountService.discount(member,10000,"fixedDiscountPolicy");

        assertThat(discountService).isInstanceOf(DiscountService.class);
        assertThat(discountPrice).isEqualTo(1000);

        int ratedDiscountPrice = discountService.discount(member, 20000, "ratedDiscountPolicy");

        assertThat(ratedDiscountPrice).isEqualTo(2000);
    }
    @Component
     static class DiscountService{
        private final Map<String,DiscountPolicy> policyMap;
        private final List<DiscountPolicy> policyList;

         @Autowired
         public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policyList) {
            this.policyMap = policyMap;
            this.policyList = policyList;
            System.out.println("policyMap = " + policyMap);
            System.out.println("policyList = " + policyList);
        }

        public int discount(Member member, int price, String discountCode) {
             DiscountPolicy discountPolicy = policyMap.get(discountCode);
             return discountPolicy.discount(member, price);
        }
    }
}
```

### 자동주입과 수동주입의 올바른 사용기준

**1. 편리한 자동기능을 기본으로 사용** 
<div class="content-box">
최근 SpringBoot는 ComponentScan을 기본으로 사용하고 SpringBoot의 다양한 Spring Bean들도 조건이 맞으면 자동으로 등록되도록 설계되어있다.<br>
설정정보를 기반으로 애플리케이션을 구성하는 부분과 실제 동작하는 부분을 명확히 나누는것이 이상적이긴 하나, 개발자 입장에서 Spring Bean 하나 등록할때 객체생성하고, 주입할 대상을 일일히 적어주는 과정은 번거롭다.
<br>결정적으로 자동 빈 등록을 사용해도 OCP,DIP정책을 모두 지킬 수 있다.
</div>

**2. 수동 빈등록은 언제 사용하면 좋을까?**

Application은 크게 업무로직과 기술지원로직으로 나뉜다. <br>
`업무로직 Bean :` 웹을 지원하는 컨트롤러, 핵심 비즈니스 로직이 있는 서비스, 데이터 계층의 로직을 처리하는 리포지토리 등이 모두 업무 로직이다. 보통 비즈니스 요구사항을 개발 할 때 추가/ 변경된다.<br>
`기술지원 Bean :` 기술적인 문제나 공통관심사(AOP)를 처리할 때 주로 사용된다. 데이터베이스 연결/공통 로그처리 처럼 업무로직을 지원하기 위한 하부기술이나 공통기술들이다. 

업무로직 -> 되도록이면 자동 빈 등록 사용! 하지만 다형성을 적극활용할때는 수동 빈 등록이 유리할 때도 있다 (Map/List...) <br>
기술지원로직 - > 수동 빈 등록 사용이 유지보수에 유리!<br>

**정리**
<div class="content-box">
자동등록을 기본으로 사용!<br>
직접 등록하는 기술지원 객체는 수동 등록<br>
다형성을 적극 활용하는 비즈니스 로직은 수동등록 고려
</div>





