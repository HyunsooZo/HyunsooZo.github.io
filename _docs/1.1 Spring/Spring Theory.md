---
title: Spring Theory
category: Spring
order: 1
---

### What's Spring?

**[Features of Spring]**

> **Spring은 Java 기반의 Open Source Framework**

 Spring은 대규모 웹 애플리케이션 개발을 위해 만들어졌으며, 간단한 Java Bean을 만들고 관리하는 데 사용. 이를 통해 개발자는 애플리케이션의 객체 지향 프로그래밍을 단순화하고, 유지 보수성을 높일 수 있음.

> **Spring은 Dependency Injection(의존성 주입) 및 Inversion of Control(제어의 역전) 기능을 제공!**

이 기능은 객체 간의 의존성을 제거하고 객체 간의 결합도를 낮추어 코드를 유연하고 테스트하기 쉬운 상태로 유지.

> **Spring은 관점 지향 프로그래밍(Aspect Oriented Programming, AOP)을 지원.** 

AOP는 애플리케이션에서 발생하는 공통적인 문제를 처리하기 위한 방법으로, 로깅, 보안, 트랜잭션 관리 등 다양한 기능을 제공.

> **Spring은 모듈화가 잘 되어 있어 필요한 모듈만 사용할 수 있음.** 

예를 들어, 웹 개발을 위한 Spring MVC 모듈, 데이터베이스 처리를 위한 Spring JDBC 모듈, 보안 기능을 위한 Spring Security 모듈 등.

> **Spring은 다양한 라이브러리와 통합이 가능**. 

예를 들어, Hibernate와 JPA를 이용한 ORM(Object Relational Mapping) 기능, MyBatis와 같은 데이터베이스 처리 라이브러리, Jackson과 같은 JSON 라이브러리 등을 Spring과 함께 사용할 수 있음.

> **IOC 컨테이너를 가진다.**

IOC (Inversion Of Control) : 제어의 역전이라는 뜻으로 본래 사용자가 가지고 있던 주도권을 스프링으로 넘기게 된다는 뜻이다. 일반적인 자바 프로그램에서는 사용자가 직접 객체를 생성하고 메소드를 호출하는 방식의 흐름이었지만 스프링에게 이를 위임함으로써 스프링에서 모든 객체를 생성, 관리하여 필요한 곳에 주입시켜주게 된다.

> **많은 필터를 가지고 있음** 

필터는 쉽게 말해, 문지기이다.
톰캣과 같은 웹 컨테이너를 관리하는 것을 필터(Filter) 라고 하고, 스프링 컨테이너를 관리하는 것은 인터셉터(Interceptor) 라고 한다. 필터는 Request/Response 객체를 조작할 수 있지만, 인터셉터는 불가능하다.
필터의 경우 전역적인 보안과 인증 작업, 이미지/데이터 압축 및 문자열 인코딩 등의 역할을 수행하고, 인터셉터는 클라이언트의 요청과 관련한 세부적인 보안과 인증 작업, API 호출에 대한 로깅, Controller로 넘겨주는 정보의 가공 등을 수행한다.

> **많은 어노테이션(@)을 가지고 있다.**

자바에서 어노테이션은 주석 그 이상의 의미를 지닌다. Annotation은 메타데이터의 일종으로 주석의 기능 뿐만 아니라 특별한 기능을 수행한다. Annotation을 활용하여 spring은 해당 클래스가 어떤 역할인지 정하기도 하고, Bean을 주입하기도 한다.

> **MessageConverter를 가지고 있다. 기본값은 현재 Json이다.**

 MessageConverter는 스프링에 기본 라이브러리로 내장되어있고, 예전에는 Xml형식의 파일로 구성했다면 현재는 JSON으로 대체되었다.

> **BufferedReader와 BufferedWriter를 쉽게 사용할 수 있다.**

Buffer는 자바 I/O를 공부했다면 한번쯤은 들어봤을 내용이다. 스프링은 버퍼를 이용해서 읽고 쓰는 함수인 BufferedReader/ BufferedWriter를 직접 구현할 필요 없이 관련 어노테이션을 제공한다 (ResponseBody(=BufferedWriter)/RequestBody(=bufferedReader)).

**[Annotation Example]**

```java
@Component
//-> Class를 Spring의 Bean으로 등록할 때 사용. (메모리에 로딩)
	@Component(value="mycar")
	public class Car {
    	public Car() {
        	System.out.println("Drive");
    	}
	}
@Autowired
//-> Bean을 주입받기 위해 사용. Spring은 class를 보고 Type에 맞게 Bean을 주입함. (Type이 없으면 Name 확인)

@Controller
// -> Spring에게 해당 Class가 Controller의 역할을 한다고 명시하기 위해 사용하는 Annotation.

@ResponseBody // -> BufferedWriter가 동작.
@RequestBody // -> BufferedReader가 동작.
```



