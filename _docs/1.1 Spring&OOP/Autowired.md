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

<div class="content-box">
<br>
그런데 <span class="emphasis">@Autowired</span>만 아용하면, <span class="emphasis">required</span> 옵션 기본값이 <span class="emphasis">true</span>로 설정되어있어 자동주입대상이 없으면 오류가 발생한다. 
</div>

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

