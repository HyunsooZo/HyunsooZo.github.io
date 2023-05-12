---
title: Singleton Pattern
category: Spring Framework
order: 4
---

### Web Application & Singleton

**Wep Application 과 Singleton**

<div class="content-box">
Spring은 태생이 기업용 온라인 서비스 기술을 지원하기 위함. <br>
대부분의 Spring Application은 web임. <br>
Web Application은 보통 여러 고객이 동시에 요청을 한다. <br>
고객트래픽이 초당 100 이 나오면 초당 100개 객체가 생성/소멸된다.(메모리낭비)<br>
해결방안은 해당 객체가 1개만 생성되고 공유되도록하면된다!! -><span class="emphasis">싱글톤패턴</span>
</div>

**Singleton Pattern이란?**

<div class="content-box">
Class의 Instance가 딱 1개만 생성되는 것을 보장하는 디자인패턴<br>
같은 객체 인스턴스를 2개이상 생성되지 못하도록 막는다. <br>
싱글톤 패턴을 적용하면, 고객의 요청이 들어올 때마다 객체를 생성하는것이 아니라<br>
이미 만들어진 객체를 공유해서 효율적으로 사용할 수 있다. 
</div>

```java
public class SingletonService {
    //1. static 영역에 객체 instance를 미리 하나 생성해 올려둔다
    private static final SingletonService instance = new SingletonService();
    
    //2. public으로 객체 인스턴스 필요시 이 static 메서드를 통해서만 조회되도록 허용한다. 
    public static SingletonService getInstance(){
        return instance;
    }
    
    //3. 생성자를 private으로 선언해서 외부에서 new키워드를 사용한 객체 생성을 막는다. 
    private SingletonService(){
    }


    //Test
        @Test
    @DisplayName("싱글톤 패턴 적용한 객체 사용")
    void singletonServiceTest(){
        SingletonService instance1 = SingletonService.getInstance();
        SingletonService instance2 = SingletonService.getInstance();
        System.out.println("instance1 = " + instance1);
        System.out.println("instance2 = " + instance2);

        assertThat(instance1).isSameAs(instance2);
    }

    //Output
    instance1 = hello.core.singleton.SingletonService@49e202ad
    instance2 = hello.core.singleton.SingletonService@49e202ad
```

|Singleton Pattern의 문제점|
|--|
|싱글톤 패턴을 구현하는 코드 자체가 많이 들어간다.|
|의존관계상 클라이언트가 구체 클래스에 의존한다 <span class="emphasis">-> DIP위반</span>|
|클라이언트가 구체클래스에 의존해 <span class="emphasis">OCP원칙을 위반할 가능성</span>이 높다|
|테스트 하기 어렵다. |
|내부 속성을 변경하거나 초기화하기 어렵다.|
|private 생성자로 자식 클래스를 만들기 어렵다.|
|유연성이 떨어진다.|
|Anti-Pattern이라고도 불린다. |


**SingletonContainer(SpringContainer)**

<div class="content-box">
Spring Container는 싱글턴패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리함. <br>

Spring Container는 Singleton Container의 역할을 한다. 이렇게 싱글톤객체를 생성하고 <br>
관리하는 기능을 싱글톤 레지스트리라고 한다. <br>

Spring Container는 이런 기능 덕분에 Singleton Pattern의 모든 단점을 해결하면서 객체를 <br>
Singleton으로 유지할 수 있다. (DIP/OCP/등의 단점으로부터 자유롭게 사용가능)
</div>

```java
    @Test
    @DisplayName("스프링컨테이너와 싱글톤")
    void SpringContainer(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberService memberService1 = ac.getBean("memberService",MemberService.class);
        MemberService memberService2 = ac.getBean("memberService",MemberService.class);

        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        assertThat(memberService1).isSameAs(memberService2);
    }

    //Output - 참조값이 같음
    memberService1 = hello.core.member.MemberServiceImpl@5c669da8
    memberService2 = hello.core.member.MemberServiceImpl@5c669da8  
```

***따라서, 스프링을 사용하면, 고객의 요청이 올 때마다 객체를 생성하는 것이 아니라, 이미 만들어진 객체를 공유하여 효율적으로 재사용 할 수 있다.***

### Singleton 방식의 주의점

<div class="content-box">
∙ 싱글톤 패던이든, 스프링 같은 싱글톤 컨테이너를 사용하든, 객체 인스턴스를 하나만 생성해서 공유하는 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하므로, <span class="emphasis">싱글톤 객체는 상태를 Stateful하게 설계하면안된다.</span><br>
∙ Stateless하게 설계해야한다!<br>
∙ Spring Bean의 feild에 공유값을 설정하면 큰 장애가 발생할 수도 있다. 
 </div>

 |Stateless 한 설계방법|
 |--|
 |특정 클라이언트에 의존적인 필드가 존재하면 안된다.|
 |특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.|
 |가급적이면 읽기만 가능해야한다.|
 |필드 대신에 자바에서 공유되지 않는, 지역변수, 파라미터, ThreadLocal 등을 사용해야한다.|


### @Configuration & Singleton


<div class="content-box">
∙ Spring Container는 Singleton Registry이다. 따라서 Spring Bean이 Singleton이 되도록 보장한다.<br>
∙ 그러나 Spring이 Java코드까지 컨트롤 할 수 는 없다.<br>
∙ 그래서 Spring은 Class의 ByteCode를 조작하는 Library(CGLIB)를 사용한다.<br>
∙ (예상) <br>
<span class="emphasis">IF</span> 호출 되었을때 이미 Spring Container에 등록된 빈?-> 컨테이너에서 호출<br>
<span class="emphasis">ELSE</span> 상속받아 새로 생성
∙ 다만 위 CGLIB는 @Configuration 을 사용하고 @Bean에 추가해야 사용된다. 
</div>

```
+----------+            +---Spring Container-----+
| AppConfig|            |                        |
+----------+            |     appConfig          |
      |                 |instance:Appconfig@CGLIB|
      |(extends)        |                        |
+----------------+      |                        |
| AppConfig@CGLIB|----> |                        |
+----------------+      +------------------------+

```
***Spring은 위 임의 클래스 AppConfig@CGLIB 가 싱글톤이 되도록 보장한다.***





