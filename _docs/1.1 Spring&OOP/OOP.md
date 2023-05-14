---
title: Spring & OOP
category: Spring Framework
order: 1
---

### What's OOP ? 

**OOP(Object-Oriented Programming)**

<div class="content-box">
∙ 현실 세계의 객체(Object) 개념을 프로그래밍에 적용하여 개발하는 방식 <br>
∙ 객체는 속성과 행위로 이루어져 있으며, 이러한 객체들이 서로 상호작용하며 프로그램을 구성<br>
∙ OOP에서는 클래스(Class)를 정의하여 객체를 생성하고 이를 이용해 프로그램을 개발
</div>

정리하자면, <br>
객체지향프로그래밍은 컴퓨터의 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러개의 독립단위, 
즉 <span class="emphasis">객체</span>들의 모임으로 파악하고자 하는것. 각각의  <span class="emphasis">객체</span>는  <span class="emphasis">메시지</span>를 주고 받고, 데이터를 처리할 수 있다. <br>

이상적으로는,<br>
***모든 설계에*** Interface를 부여하는 것이 좋다. 

**Features Of OOP**

| OOP Features||
|--|--|
|캡슐화(Encapsulation)| 객체의 속성과 행위를 하나로 묶어 은닉화하여 객체를 보호 |
|상속성(Inheritance)| 객체의 공통된 속성과 행위를 상위 클래스에서 정의하고, 하위 클래스에서 상속하여 사용|
|다형성(Polymorphism)| 하나의 객체가 다양한 형태로 사용될 수 있음|
|추상화(Abstraction)| 객체의 공통적인 특징을 추출하여 일반화시키는 것|

### SOLID OOP ? 

|SOLID|My Understanding|
|--|--|
|**S**RP(단일책임원칙)<br>[분류]|하나의 클래스는 하나의 책임만<br>**컨플릭 방지, 역할에 해당하는 서비스 잘찾기**|
|**O**CP(개방폐쇄원칙)<br>[교체]|확장에는 열려있고, 변경에는 닫혀있다<br>**if else에서 반복적인 케이스가 보이면, 클래스 분리고려**|
|**L**SP(리스코프치환원칙)<br>[교체]|상속받은 클래스는 부모클래스와 동일한 동작을 해야 재활용 가능성 ↑<br>**상속보다는 I/F고려하고, 상속을 하더라도 비슷하게 만들어야..**|
|**I**SP(인터페이스분리원칙)<br>[분류]|Inteface도 단일 책임 갖도록 분리<br>**인터페이스도 OCP를 따라야 구현 편리/재활용 용이**|
|**D**IP(의존성역전원칙)<br>[교체]|하위모듈이 상위모듈에 의존하도록..<br>**중간 I/F를 둬야 하위 모듈 변경이 쉬워짐**|



### What is Spring?

<div class="content-box">
Spring은 Java 기반의 오픈 소스 애플리케이션 프레임워크로, 개발자들이 애플리케이션을 보다 쉽게 개발하고 유지보수할 수 있도록 다양한 기능을 제공. 대표적으로  <span class="emphasis">Dependency Injection(DI)</span>과  <span class="emphasis">Inversion of Control(Ioc)</span> 등을 제공하고<span class="emphasis">객체 지향적인 설계</span>와 개발을 촉진하고 지원.<br><br>
스프링은 객체를 생성하고 조립하는데 있어서 위에 언급한 DI 이라는 기술을 사용하며, 이를 통해  <span class="emphasis">객체간의 결합도를 낮추고 모듈화된 구조</span>를 갖출 수 있음. 또한, 스프링은 객체 지향적인 설계를 위한 다양한 패턴과 기능을 제공하며, 이를 통해 애플리케이션의 확장성과 유연성을 높일 수 있음. 따라서, 스프링은 <u><b>객체 지향 프로그래밍에 매우 적합한 프레임워크</b></u>이며, 객체 지향적인 설계와 개발을 위한 다양한 기능과 도구를 제공한다.
</div>


### Features of Spring

**∙ Spring은 Dependency Injection(의존성 주입) 및 Inversion of Control(제어의 역전) 기능을 제공!**
<div class="content-box">
이 기능은 객체 간의 의존성을 제거하고 객체 간의 결합도를 낮추어 코드를 유연하고 테스트하기 쉬운 상태로 유지.
</div>

**∙ Spring은 관점 지향 프로그래밍(Aspect Oriented Programming, AOP)을 지원.**
<div class="content-box">
AOP는 애플리케이션에서 발생하는 공통적인 문제를 처리하기 위한 방법으로, 로깅, 보안, 트랜잭션 관리 등 다양한 기능을 제공.
</div>

**∙ Spring은 모듈화가 잘 되어 있어 필요한 모듈만 사용할 수 있음.**
<div class="content-box">
예를 들어, 웹 개발을 위한 Spring MVC 모듈, 데이터베이스 처리를 위한 Spring JDBC 모듈, 보안 기능을 위한 Spring Security 모듈 등.
</div>

**∙ Spring은 다양한 라이브러리와 통합이 가능**
<div class="content-box">
예를 들어, Hibernate와 JPA를 이용한 ORM(Object Relational Mapping) 기능, MyBatis와 같은 데이터베이스 처리 라이브러리, Jackson과 같은 JSON 라이브러리 등을 Spring과 함께 사용할 수 있음.
</div>

**∙ IOC 컨테이너를 가진다.**
<div class="content-box">
IOC (Inversion Of Control) : 제어의 역전이라는 뜻으로 본래 사용자가 가지고 있던 주도권을 스프링으로 넘기게 된다는 뜻이다. 일반적인 자바 프로그램에서는 사용자가 직접 객체를 생성하고 메소드를 호출하는 방식의 흐름이었지만 스프링에게 이를 위임함으로써 스프링에서 모든 객체를 생성, 관리하여 필요한 곳에 주입시켜주게 된다.
</div>

**∙ 많은 필터를 가지고 있음**
<div class="content-box">
필터는 쉽게 말해, 문지기이다.<br>톰캣과 같은 웹 컨테이너를 관리하는 것을 필터(Filter) 라고 하고, 스프링 컨테이너를 관리하는 것은 인터셉터(Interceptor) 라고 한다. <br>필터는 Request/Response 객체를 조작할 수 있지만, 인터셉터는 불가능하다.필터의 경우 전역적인 보안과 인증 작업, 이미지/데이터 압축 및 문자열 인코딩 등의 역할을 수행하고, 인터셉터는 클라이언트의 요청과 관련한 세부적인 보안과 인증 작업, API 호출에 대한 로깅, Controller로 넘겨주는 정보의 가공 등을 수행한다.
</div>

**∙ 많은 Annotation(@)을 가지고 있다.**
<div class="content-box">
자바에서 어노테이션은 주석 그 이상의 의미를 지닌다. Annotation은 메타데이터의 일종으로 주석의 기능 뿐만 아니라 특별한 기능을 수행한다. Annotation을 활용하여 spring은 해당 클래스가 어떤 역할인지 정하기도 하고, Bean을 주입하기도 한다.
</div>

**∙ MessageConverter를 가지고 있다. 기본값은 현재 Json이다.**
<div class="content-box">
MessageConverter는 스프링에 기본 라이브러리로 내장되어있고, 예전에는 Xml형식의 파일로 구성했다면 현재는 JSON으로 대체되었다.
</div>

**∙ BufferedReader와 BufferedWriter를 쉽게 사용할 수 있다.**
<div class="content-box">
Buffer는 자바 I/O를 공부했다면 한번쯤은 들어봤을 내용이다. 스프링은 버퍼를 이용해서 읽고 쓰는 함수인 BufferedReader/ BufferWriter를 직접 구현할 필요 없이 관련 어노테이션을 제공한다 (ResponseBody(=BufferedWriter)/RequestBody(=bufferedReader)).
</div>


### Lego & Spring 

|**∙ Lego의 편리한 점**|**∙ Spring의 좋은 점**|
|--|--|
|바닥에 넓은 판을 두고<br>그 위에 이것 저것 얹으면 편하다. |Spring이라는 넓은 판(Container)위에 <br>내가 만드는 Class만 얹으면 개발이 됨 |
|동그란 끼우는 부분이 모두 동일하므로<br>어떤 것들이든 서로 끼워볼 수 있다.|Spring Bean이라는 규격에 맞추어<br> 만들면 서로 가져다쓰기가 좋다. |
|너무 높은 자유도가 없기때문에<br> 오히려 무언가를 만들기가 편하다.| 너무 자유롭지 않기때문에 원리를<br> 알면 그 틀 안에서 작업이 편하다 |
