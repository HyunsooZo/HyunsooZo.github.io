---
title: Spring 
category: Spring & OOP
order: 2
---

### What's Spring?

**[Features of Spring]**

**∙ Spring은 Java 기반의 Open Source Framework**

<div style="border: 0.1px solid white; padding: 5px;">
Spring은 대규모 웹 애플리케이션 개발을 위해 만들어졌으며, 간단한 Java Bean을 만들고 관리하는 데 사용. 이를 통해 개발자는 애플리케이션의 객체 지향 프로그래밍을 단순화하고, 유지 보수성을 높일 수 있음.
</div><br>

**∙ Spring은 Dependency Injection(의존성 주입) 및 Inversion of Control(제어의 역전) 기능을 제공!**
<div style="border: 0.1px solid white; padding: 5px;">
이 기능은 객체 간의 의존성을 제거하고 객체 간의 결합도를 낮추어 코드를 유연하고 테스트하기 쉬운 상태로 유지.
</div><br>

**∙ Spring은 관점 지향 프로그래밍(Aspect Oriented Programming, AOP)을 지원.**
<div style="border: 0.1px solid white; padding: 5px;">
AOP는 애플리케이션에서 발생하는 공통적인 문제를 처리하기 위한 방법으로, 로깅, 보안, 트랜잭션 관리 등 다양한 기능을 제공.
</div><br>

**∙ Spring은 모듈화가 잘 되어 있어 필요한 모듈만 사용할 수 있음.**
<div style="border: 0.1px solid white; padding: 5px;">
예를 들어, 웹 개발을 위한 Spring MVC 모듈, 데이터베이스 처리를 위한 Spring JDBC 모듈, 보안 기능을 위한 Spring Security 모듈 등.
</div><br>

**∙ Spring은 다양한 라이브러리와 통합이 가능**
<div style="border: 0.1px solid white; padding: 5px;">
예를 들어, Hibernate와 JPA를 이용한 ORM(Object Relational Mapping) 기능, MyBatis와 같은 데이터베이스 처리 라이브러리, Jackson과 같은 JSON 라이브러리 등을 Spring과 함께 사용할 수 있음.
</div><br>

**∙ IOC 컨테이너를 가진다.**
<div style="border: 0.1px solid white; padding: 5px;">
IOC (Inversion Of Control) : 제어의 역전이라는 뜻으로 본래 사용자가 가지고 있던 주도권을 스프링으로 넘기게 된다는 뜻이다. 일반적인 자바 프로그램에서는 사용자가 직접 객체를 생성하고 메소드를 호출하는 방식의 흐름이었지만 스프링에게 이를 위임함으로써 스프링에서 모든 객체를 생성, 관리하여 필요한 곳에 주입시켜주게 된다.
</div><br>

**∙ 많은 필터를 가지고 있음**
<div style="border: 0.1px solid white; padding: 5px;">
필터는 쉽게 말해, 문지기이다.<br>톰캣과 같은 웹 컨테이너를 관리하는 것을 필터(Filter) 라고 하고, 스프링 컨테이너를 관리하는 것은 인터셉터(Interceptor) 라고 한다. <br>필터는 Request/Response 객체를 조작할 수 있지만, 인터셉터는 불가능하다.필터의 경우 전역적인 보안과 인증 작업, 이미지/데이터 압축 및 문자열 인코딩 등의 역할을 수행하고, 인터셉터는 클라이언트의 요청과 관련한 세부적인 보안과 인증 작업, API 호출에 대한 로깅, Controller로 넘겨주는 정보의 가공 등을 수행한다.
</div><br>

**∙ 많은 Annotation(@)을 가지고 있다.**
<div style="border: 0.1px solid white; padding: 5px;">
자바에서 어노테이션은 주석 그 이상의 의미를 지닌다. Annotation은 메타데이터의 일종으로 주석의 기능 뿐만 아니라 특별한 기능을 수행한다. Annotation을 활용하여 spring은 해당 클래스가 어떤 역할인지 정하기도 하고, Bean을 주입하기도 한다.
</div><br>

**∙ MessageConverter를 가지고 있다. 기본값은 현재 Json이다.**
<div style="border: 0.1px solid white; padding: 5px;">
MessageConverter는 스프링에 기본 라이브러리로 내장되어있고, 예전에는 Xml형식의 파일로 구성했다면 현재는 JSON으로 대체되었다.
</div><br>

**∙ BufferedReader와 BufferedWriter를 쉽게 사용할 수 있다.**
<div style="border: 0.1px solid white; padding: 5px;">
Buffer는 자바 I/O를 공부했다면 한번쯤은 들어봤을 내용이다. 스프링은 버퍼를 이용해서 읽고 쓰는 함수인 BufferedReader/ BufferWriter를 직접 구현할 필요 없이 관련 어노테이션을 제공한다 (ResponseBody(=BufferedWriter)/RequestBody(=bufferedReader)).
</div><br>

### Lego & Spring 

|**∙ Lego를 만들 때 편리한 점**|**∙ Spring을 쓰면 좋은 점**|
|--|--|
|바닥에 넓은 판을 두고<br>그 위에 이것 저것 얹으면 편하다. |Spring이라는 넓은 판(Container)위에 <br>내가 만드는 Class만 얹으면 개발이 됨 |
|동그란 끼우는 부분이 모두 동일하므로<br>어떤 것들이든 서로 끼워볼 수 있다.|Spring Bean이라는 규격에 맞추어<br> 만들면 서로 가져다쓰기가 좋다. |
|너무 높은 자유도가 없기때문에<br> 오히려 무언가를 만들기가 편하다.| 너무 자유롭지 않기때문에 원리를<br> 알면 그 틀 안에서 작업이 편하다 |


### DI

**DI : Dependency Injection**

핵심 : <font color="orange">*(A가 B를 사용한다 = A가 B에 의존한다)*</font>

∙Dependency Injection(DI)는 객체 지향 프로그래밍에서의 의존성(dependency)을 관리하는 디자인 패턴 중 하나다. 의존성이란, 한 객체가 다른 객체에 의존하는 관계를 말하고 예를 들면 클래스 A가 클래스 B를 사용한다면, 클래스 A는 클래스 B에 의존하는것으로 본다.

∙DI는 객체 생성 시점에 의존하는 객체를 주입(injection)해주는 방법을 사용하여 의존성을 관리한다. 이를 통해 객체 간의 결합도(coupling)를 낮출 수 있다. 결합도가 높다는 것은, 한 객체를 변경하면 이를 사용하는 다른 객체도 변경되어야 한다는 것을 의미하며 이는 코드의 유지보수성을 떨어뜨리고, 개발자의 작업량을 늘리는 원인이 된다.

∙DI를 사용하면, 객체 간의 결합도를 낮출 수 있으므로 코드의 유지보수성과 재사용성을 높일 수 있다. 또한, 테스트하기 쉬운 코드를 작성할 수 있다. 예를 들어, DI를 사용하면 테스트를 위해 가짜 객체(mock object)를 사용하여 테스트할 수 있으므로, 실제 객체를 사용하지 않아도 테스트할 수 있다.

|DI 방식| 설명|
|--|--|
|생성자 주입<br>(Constructor Injection)|  객체 생성 시점에 의존 객체를 전달하는 방식<br><font color="orange">****생성자 주입 방식이 주로 사용됨***</font>|
|Setter 주입<br>(Setter Injection)|Setter 메서드를 통해 의존 객체를 전달하는 방식|
|인터페이스 주입<br>(Interface Injection)| 인터페이스를 통해 의존 객체를 전달하는 방식|

*-* ***xml을 통해 Bean을 등록하는 방식은 현재 거의 사용되지 않고 있다.***

*-* ***Java Configuration을 이용한 Bean 등록을 하기도 한다***

*-* ***<u><font color="orange">Annotation</font>을 이용한 Bean 등록 및 DI가 가장 많이, 자주 쓰인다.</u>***

**[Commonly used Annotations]**
```java
@Autowired // 의존성 주입에 사용되며, 해당 클래스의 생성자, 필드, 메서드의 파라미터에 적용됨.
@Component // Bean으로 등록될 클래스에 적용됨.
@Repository // 데이터베이스와 연동되는 DAO(Data Access Object) 클래스에 적용됨.
@Service // 비즈니스 로직을 처리하는 Service 클래스에 적용됨.
@Controller // 웹 애플리케이션에서 클라이언트 요청을 처리하는 컨트롤러 클래스에 적용됨.
@RequestMapping // HTTP 요청 URL과 Controller 메서드를 매핑하기 위해 사용됨.
@GetMapping // HTTP GET 요청 URL과 Controller 메서드를 매핑하기 위해 사용됨.
@PostMapping // HTTP POST 요청 URL과 Controller 메서드를 매핑하기 위해 사용됨.
@PutMapping // HTTP PUT 요청 URL과 Controller 메서드를 매핑하기 위해 사용됨.
@DeleteMapping // HTTP DELETE 요청 URL과 Controller 메서드를 매핑하기 위해 사용됨.
@PathVariable // URL 경로에서 변수를 추출하기 위해 사용됨.
@RequestParam // HTTP 요청 파라미터를 추출하기 위해 사용됨.
@ResponseBody // Controller 메서드에서 반환되는 값을 HTTP 응답 Body에 직접 적용하기 위해 사용됨.
@Transactional // 트랜잭션 처리를 위해 사용됨.
@Configuration // Spring의 설정 클래스임을 나타내며, Bean을 등록할 때 사용됨.
@EnableAutoConfiguration // Spring Boot의 자동 설정을 활성화하기 위해 사용됨.
@SpringBootApplication // Spring Boot 애플리케이션임을 나타내며, @Configuration, @EnableAutoConfiguration, @ComponentScan을 모두 포함하고 있음

```

### Bean 관련 설정방법

**∙ Bean의 구현체가 여러개인 경우 주입 받는 방법**

**1.** <font color="orange">@Primary</font> -> 해당 빈을 최우선으로 주입

**2.** <font color="orange">@Qualifier("beanName")</font> -> beanName으로 지정된 빈 주입

**3.** <font color="orange">Set</font> 또는 <font color="orange">List</font>로 받기

**4.** <font color="orange">Property</font> 이름을 <font color="orange">bean</font>과 동일하게하기. (**<u>가장 흔히 사용</u>**)

**∙ Bean 의 Scope**

<font color="orange">Singleton</font> : 일반적인 방법, 하나만 만들어 계속 재활용

**∙ Prototype: 매번 새로만드는 방법 (데이터 클리어 필요 시)**

<font color="orange">Request</font> : 요청에 따라 계속 새로 만듦

<font color="orange">Session</font> : 세션마다 계속 새로 만듦

<font color="orange">WebSocket</font> : 양방향 실시간 통신

**∙Spring의 환경설정 : profile**

∙ 현업에서는 환경을 다양하게 하여 해당 환경에서만 동작하는 Bean을 만드는 경우가있음. 

∙ 클래스단위에 정용하거나 메서드 단위에 적용가능 

```java
//클래스 단위
   @Configuration @Profile("test") 
   @Component @Profile("test") 

//메서드 단위
   @Bean @Profile("test")
```

∙ 프로파일 표현식

``@Profile("!production")``

``!not,&and,|or``


### IoC(Inversion Of Control)


**∙** 제어의 역전(IoC, Inversion of Control)는 소프트웨어 디자인 패턴 중 하나. 일반적으로 프로그램에서 객체 간의 의존성은 직접적으로 코드 내부에서 구현. 하지만 IoC는 이와 반대로, 객체 간의 의존성을 외부에서 관리하도록 구현.

**∙** 즉, IoC는 객체 생성, 의존성 주입, 객체 생명주기 관리 등의 일련의 작업을 프레임워크 또는 컨테이너가 대신 처리하도록 구현하는 것. 이렇게 구현하면, 개발자는 객체 간의 의존성을 관리하는 코드를 직접 작성하지 않아도 되므로, 코드의 가독성과 유지보수성이 높아짐.

**∙** IoC는 대표적으로 Spring Framework, Google Guice, Dagger2 등의 프레임워크에서 지원되는 기능. 이러한 프레임워크를 사용하면 객체 생성, 의존성 주입, 객체 생명주기 관리 등을 프레임워크가 대신 처리해주므로 개발자는 비즈니스 로직에 집중할 수 있음.


**∙** [예시]

일반적인 코드에서는 객체 A가 객체 B를 생성하고, 객체 A에서 객체 B의 메서드를 호출하는 식으로 제어의 흐름이 이루어짐. 
하지만 <font color="orange">**IoC에서는 객체 B를 외부에서 생성하여 객체 A에게 전달하고, 객체 A는 이를 사용하여 로직을 처리.**</font> 
이렇게 되면, 객체 A는 객체 B에 의존하지만, 객체 B는 객체 A에 대한 의존성을 갖지 않음. 이렇게 구현하면, 객체 간의 결합도를 낮추고 유지보수성과 테스트 용이성을 높일 수 있음.


### Spring의 부가기능

**1. Resource 가져오기**

Java의 표준 클래스들은 다양한 Resource에 접근할 때 충분한 기능을 제공하지 않음. 스프링은 필요한 기능을 만들어서 제공함. 

**[Spring에서 제공하는 resource관련 함수들]**
```java
public interface Resource extends InputStreamSource{

    boolean exists();
    boolean isReadable();
    boolean isOpen();
    boolean isFile();
    URL getURL() throws IOExeption;
    URI getURI() throws IOExeption;
    File getFile() throws IOExeption;
    ReadableByteChannel readableChannel() throws IOExeption;
    long contentLength() throws IOExeption;
    long lastModified() throws IOException;
    Resource createRelative(String relativePath) throws IOException;
    String getFilename();
    String getDescription();
}
```

**Resource 구현체 목록**

**∙ URLResource**

java.net.URL 을 래핑한 버전. 다양한 종류(ftp:,file:,http: 등의 prefix로 접근유형을 판단) 의 Resource에 접근 가능하지만 <u>기본적으로는 http(s)로 원격접근</u>

**∙ ClassPathResource**

classPath(소스코드를 빌드한 결과(기본적으로 target/classes폴더)) 하위의 리소스 접근 시 사용

**∙ FileSystemResource**

이름과 같이 file을 다루기 위한 리소스 구현체

**∙ ServletContextResource,InputStreamResource,ByteArrayResource**

servlet 어플리케이션 루트 하위 파일, InputStream,ByteArrayInput 스트림을 가져오기 위한 구현체





