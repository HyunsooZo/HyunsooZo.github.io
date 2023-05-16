---
title: DI & IoC
category: Spring Framework
order: 2
---

### Dependency Injection

**DI : Dependency Injection**

`(A가 B를 사용한다 = A가 B에 의존한다)`

∙Dependency Injection(DI)는 객체 지향 프로그래밍에서의 의존성(dependency)을 관리하는 디자인 패턴 중 하나다. 의존성이란, 한 객체가 다른 객체에 의존하는 관계를 말하고 예를 들면 클래스 A가 클래스 B를 사용한다면, 클래스 A는 클래스 B에 의존하는것으로 본다.

∙DI는 객체 생성 시점에 의존하는 객체를 주입(injection)해주는 방법을 사용하여 의존성을 관리한다. 이를 통해 객체 간의 결합도(coupling)를 낮출 수 있다. 결합도가 높다는 것은, 한 객체를 변경하면 이를 사용하는 다른 객체도 변경되어야 한다는 것을 의미하며 이는 코드의 유지보수성을 떨어뜨리고, 개발자의 작업량을 늘리는 원인이 된다.

∙DI를 사용하면, 객체 간의 결합도를 낮출 수 있으므로 코드의 유지보수성과 재사용성을 높일 수 있다. 또한, 테스트하기 쉬운 코드를 작성할 수 있다. 예를 들어, DI를 사용하면 테스트를 위해 가짜 객체(mock object)를 사용하여 테스트할 수 있으므로, 실제 객체를 사용하지 않아도 테스트할 수 있다.

|DI 방식| 설명|
|--|--|
|생성자 주입<br>(Constructor Injection)|  객체 생성 시점에 의존 객체를 전달하는 방식<br>`생성자 주입 방식이 주로 사용됨`|
|Setter 주입<br>(Setter Injection)|Setter 메서드를 통해 의존 객체를 전달하는 방식|
|인터페이스 주입<br>(Interface Injection)| 인터페이스를 통해 의존 객체를 전달하는 방식|

*-* ***xml을 통해 Bean을 등록하는 방식은 현재 거의 사용되지 않고 있다.***

*-* ***Java Configuration을 이용한 Bean 등록을 하기도 한다***

*-* ***<u>`Annotation`을 이용한 Bean 등록 및 DI가 가장 많이, 자주 쓰인다.</u>***

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

### IoC(Inversion Of Control)

`내가 아니라 프레임워크가 내 코드를 대신 호출하도록 한다!`

<div class="content-box">
제어의 역전(IoC, Inversion of Control)는 소프트웨어 디자인 패턴 중 하나. 일반적으로 프로그램에서 객체 간의 의존성은 직접적으로 코드 내부에서 구현. 하지만 IoC는 이와 반대로, <span class="emphasis">객체 간의 의존성을 외부에서 관리하도록 구현.</span>
</div>

<div class="content-box">
즉, IoC는 객체 생성, 의존성 주입, 객체 생명주기 관리 등의 일련의 작업을 <span class="emphasis">프레임워크 또는 컨테이너가 대신 처리하도록 구현하는 것.</span> 이렇게 구현하면, 개발자는 객체 간의 의존성을 관리하는 코드를 직접 작성하지 않아도 되므로, 코드의 가독성과 유지보수성이 높아짐.
</div>

<div class="content-box">
IoC는 대표적으로 Spring Framework, Google Guice, Dagger2 등의 프레임워크에서 지원되는 기능. 이러한 프레임워크를 사용하면 객체 생성, 의존성 주입, 객체 생명주기 관리 등을 프레임워크가 대신 처리해주므로 개발자는 비즈니스 로직에 집중할 수 있음.
</div>

<div class="content-box">
일반적인 코드에서는 객체 A가 객체 B를 생성하고, 객체 A에서 객체 B의 메서드를 호출하는 식으로 제어의 흐름이 이루어짐. 
하지만 <u>`IoC에서는 객체 B를 외부에서 생성하여 객체 A에게 전달하고, 객체 A는 이를 사용하여 로직을 처리.` 
이렇게 되면, 객체 A는 객체 B에 의존하지만, 객체 B는 객체 A에 대한 의존성을 갖지 않음.</u> 이렇게 구현하면, 객체 간의 결합도를 낮추고 유지보수성과 테스트 용이성을 높일 수 있음.
</div>


### IoC Container (DI Contaniner)

<div class="content-box">
객체를 생성하고 관리하면서 의존관계를 연결해주는 것을 `IoC Container` 또는 `DI Container`라고 한다. 
의존관계 주입에 초점을 맞추어 최근에는 주로 `DI Container`라고 한다. 
또는 Assembler, Object Factory 라고도 불린다. 
</div>

