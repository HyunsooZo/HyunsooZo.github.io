---
title: Spring 북스터디 5~8주차 
category: ZeroBase BootCamp
order: 3
---
> 제로베이스 백엔드 부트캠프 - 북스터디 (마스터반)

### 리딩 5주차 

09장: 연관관계 매핑

***연관관계 매핑 종류와 방향***

|매핑|방향|
|--|--|
|One To One | 일대일(1:1)|
|One To Many | 일대다(1:N)|
|Many To One | 다대일(N:1)|
|Many To Many | 다대다(N:M)|
|단방향 | 두 엔티티의 관계에서 한쪽의 엔티티만 참조하는 형식|
|양방향 | 두 엔티티의 관계에서 각 엔티티가 서로의 엔티티를 참조하는 형식|

##### 일대일(1:1) 매핑

**1:1 단방향 매핑**
```java
//다른 엔티티 객체를 필드로 정의했을 때 일대일 연관관계로 매핑하기 위해
@OneToOne
// 매핑할 외래키 설정
@JoinColumn(name = "ex")
private Example example;
```
`referencedColumnName` : 외래키가 참조할 상대 테이블의 칼럼명 지정

`foreignKey` : 외래키를 생성하면서 지정할 제약조건(unique, nullable, insertable, updatable) 설정

**1:1 양방향 매핑**
```java
@OneToOne(mappedBy = "ex")
private Example example;
```
`mappedBy` : 어떤 객체가 ***주인인지*** 표시하는 속성<br>엔티티는 양방향으로 매핑하되 한쪽에게만 외래키를 주기 위해 사용되는 속성 값<br>
mappedBy에 들어가는 값은 연관관계를 갖고 있는 상대 엔티티에 있는 연관관계 필드의 이름

##### N:1, 1:N 매핑
```java
//다대일 단방향 매핑
@ManyToOne
@JoinColumn(name = "example")
@ToString.Exclude
```
**N:1 양방향 매핑**
```java
@OneToMany(mappedBy = "example", fetch = FetchType.EAGER)
@ToString.Exclude
private List<Ex> exList = new ArrayList<>();
```
**1:N 단방향 매핑**
```java
//@OneToMany와 @JoinColumn 사용 시, 별도의 설정 없이도 일대다 단방향 연관관계 매핑
@OneToMany(fetch = FetchType.EAGER)
@JoinColumn(name = "example")
private List<Example> examples = new ArrayList<>();
```
##### N:N 매핑
<div class="content-box">
각 엔티티에서 서로를 리스트로 가지는 구조<br>
교차 엔티티라고 부르는 중간 테이블을 생성하여 다대다 관계를 일대다 또는 다대일 관계로 해소.
</div>

**N:N 단방향 매핑**
리스트로 필드를 가지는 객체에서는 외래키를 가지지 않으므로 별도의 `@JoinColumn` 설정 불필요
```java
@ManyToMany
@ToString.Exclude
private List<Example> examples = new ArrayList<>();
```
**N:N 양방향 매핑**
필요에 따라 `mappedBy` 속성을 사용해 두 엔티티 간 연관관계의 주인 설정 가능
```java
@ManyToMany
@ToString.Exclude
private List<Example> examples = new ArrayList<>();
```
```java
//양방향 연관관계 설정을 위한 코드
ex1.addExample(example1);
ex2.addExample(example1);
ex2.addExample(example2);
ex3.addExample(example2);
```
#####  영속성 전이

|영속성 전이<br>(cascade)| 특정 엔티티의 영속성 상태를 변경할 때 그 엔티티와 연관된 엔티티의 영속성에도 영향을 미쳐 영속성 상태를 변경하는 것|
|-|-|
|ALL | 모든 영속 상태 변경에 대해 영속성 전이 사용|
|PERSIST| 엔티티가 영속화할 때 연관된 엔티티도 함께 영속화|
|MERGE | 엔티티를 영속성 컨텍스트에 병합할 때 연관된 엔티티도 병합|
|REMOVE| 엔티티를 제거할 때 연관된 엔티티도 제거|
|REFRESH | 엔티티를 새로고침할 때 연관된 엔티티도 새로고침|
|DETACH | 엔티티를 영속성 컨텍스트에서 제외하면 연관된 엔티티도 제외|

***1. 영속성 전이 적용***<br>
영속 상태의 변화에 따라 연관된 엔티티들의 동작도 함께 수행할 수 있어 개발의 생산성 향상
```java
//@OneToMany 어노테이션 속성 활용
@OneToMany(mappedBy = "example", cascade = CascadeType.PERSIST)
@ToString.Exclude
private List<Example> exampleList = new ArrayList<>();
```

***2. 고아 객체***<br>
부모 엔티티와 연관관계가 끊어진 엔티티

```java
//고아 객체를 자동으로 제거
@OneToMany(mappedBy = "example", cascade = CascadeType.PERSIST, orphanRemoval = true)
@ToString.Exclude
private List<Example> exampleList = new ArrayList<>();
```

### 리딩 6주차

10장: 유효성 검사와 예외 처리

**유효성 검사란?**
<div class="content-box">
어플리케이션 비즈니스 로직이 제대로 동작하도록 데이터를 사전에 검증<br>
여러 계층에서 들어오는 데이터에 대해 의도한 형식대로 값이 들오는지 체크하는 작업
</div>

`Bean Validation` :
<div class="content-box">
일반적인 어플리케이션 데이터 검증 시 문제점이 다소 존재<br> 
1. 계층별로 진행하는 유효성 검사는 검증 로직이 각 클래스 별로 분산되어 있어 관리 어려움<br>
2. 검증 로직에 중복이 많아 여러곳에 유사한 기능의 코드존재 가능성 있음<br>
3. 검증대상에 따라 검증 코드가 길어질 수 있음.(코드가독성 저하)<br>

반면 <span class="emphasis">Bean Validation</span>은<br> 
어노테이션을 통한 다양한 데이터 검증기능 제공<br>
유효성 검사를 위한 로직을 DTO 같은 도메인 모델과 묶어서 각 계층에서 사용하면서 검증 자체를 도메인 모델에 얹는 방식으로 수행<br>
어노테이션을 사용한 검증방식으로 코드 가독성 향상
</div>


##### 스프링 부트에서의 유효성 검사

**SpringBoot 유효성 검사**
<div class="content-box">
유효성 검사는 <u>각 계층으로 데이터가 넘어오는 시점에</u> 해당 데이터에 대한 검사진행<br>
일반적으로 DTO(model)객체를 대상으로 유효성 검사 수행
</div>

|스프링부트 유효성검사||
|--|--|
|Hibernate Validator| Bean Validation의 구현체인 Hibernate Validator를 표준으로 사용<br>스프링 부트 2.3 이후로 별도 의존성 추가필요 (spring-boot-starter-validation)|

##### Annotations

**문자열**

`@Null` : null값 <br>
`@NotNull` : ~~null값~~ , ""(빈값), " "(공백) <br> 
`@NotEmpty` : ~~null, ""~~ , " "(공백) <br> 
`@NotBlank`: ~~~null, "", " "~~~ <br>
`@Size(min = $number1, max = $number2)` : $number1 이상, $number2 이하의 범위 허용<br>

**최댓값/최솟값**

`@DecimalMax(value = "$numberString")` : $numberString보다 작은 값 허용<br>
`@DecimalMin(value = "$numberString")` : $numberString보다 큰 값 허용<br>
`@Min(value = $number)` : $number 이상의 값 허용<br>
`@Max(value = $number)` : $number 이하의 값 허용<br>

**값의 범위**

`@Positive` : 양수 허용<br>
`@PositiveOrZero` : 0을 포함한 양수 허용<br>
`@Negative` : 음수 허용<br>
`@NegativeOrZero` : 0을 포함한 음수 허용<br>

**시간**


`@Future` : 현재보다 미래의 날짜 허용<br>
`@FutureOrPresent` : 현재를 포함한 미래의 날짜 허용<br>
`@Past` : 현재보다 과거의 날짜 허용<br>
`@PastOrPresent` : 현재를 포함한 과거의 날짜 허용<br>

**이메일 형식**

`@Email` : 이메일 형식 체크<br>

**자릿수**

`@Digits(integer = @number1, fraction = $number2)` : $number1 -> 정수 자릿수, $number2 -> 소수 자릿수<br>

**Boolean 검증**

`@AssertTrue` : true인지 체크, null 체크 x <br>
`@AssertFalse` : false인지 체크, null 체크 x<br>

**정규식**
`@Pattern(regexp = "$expression")` : 정규식 검사<br>

|정규식 요소||
|--|--|
|^ | 문자열의 시작|
|$ | 문자열의 종료|
|. | 임의의 한 문자|
|* | 앞문자가 하나 이상|
|? | 앞문자가 없거나 하나 존재|
|[,] | 문자의 집합이나 범위, 두문자의 사이는 '-' 기호로 범위 표현|
|{,} | 횟수 또는 범위|
|(,) | 괄호안의 문자를 하나의 문자로 인식|
|| | 패턴 안에서 OR 연산 수행|
|\ | 정규식에서 역슬래시는 확장문자로 취급, 역슬래시 다음 특수문자는 문자로 인식|
|\b | 단어의 경계|
|\B | 단어가 아닌 것에 대한 경계|
|\A | 입력의 시작 부분|
|\G | 이전 매치의 끝|
|\Z | 종결자가 있는 경우 입력의 끝|
|\z | 입력의 끝|
|\s | 공백 문자|
|\S | 공백문자가 아닌 나무지 문자 (^\s와 동일)|
|\w | 알파벳이나 숫자|
|\W | 알파벳이나 숫자가 아닌 문자(^\w와 동일)|
|\d | 숫자 [0-9]와 동일하게 취급|
|\D | 숫자를 제외한 모든 문자(^0-9와 동일)|

##### Valid Annotation

`@Valid` **&** `@Validated`
<div class="content-box">
일반적으로 컨트롤러에서 유효성 검사를 하는 DTO를 @RequestBody로 받는 인자에 @Valid 어노테이션 붙여야 DTO객체에 대해 유효성 검사를 수행.<br>
@Validated(Spring제공)는 @Valid(Java제공) 기능을 포함하므로 @Validated로 대체가능 (리스트로 다그룹 지정 가능)<br>
@Validated는 유효성 검사를 그룹으로 묶어 대상 특정화 기능 존재
</div>

`@Validated`**그룹화 예시**
```java
//DTO
ex) @Min(value=20, groups = ValidationGrop1.class)
private String name;

//Controller
public ResponseEntity<?> checkValidation(
	@Validated({ValidationGroup1.class, ValidationGroup2.class}) 
	@RequestBody ValidatedReqDto dto) {...}
```
 
**커스텀 Validation 추가**

`ConstraintValidator`와 커스텀 어노테이션을 조합하여 별도의 유효성 검사 어노테이션 생성가능. <br>
동일한 정규식을 계속 쓰는 @Pattern 어노테이션의 경우가 일반적으로 사용됨.

*<전화번호 형식이 일치하는지 확인하는 유효성 검사 어노테이션 예시>*

*1. TelephoneValidator 클래스를 ContraintValidator 인터페이스의 구현체로 정의*
*2. 인터페이스 선언 시 어노테이션 인터페이스 타입 정의*
*3. isValid() 메소드 재정의*
*4. false가 리턴 되면 MethodArgumentNotValid 예외 발생*

```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = TelephoneValidator.class)
public @interface Telephone{
	String message() default "전화번호 형식이 일치하지 않습니다.";
	Class[] groups() default {};
	Class[] payload() default{};
}
```
|관련 Annotations||
|--|--|
|@Target|이 어노테이션을 어디서 선언할 수 있는지 정의 (여기선 필드에서 선언 가능하도록 설정)<br>EntityType.PACKAGE,<br>EntityType.TYPE,<br>EntityType.CONTRUCTOR,<br>EntityType.FIELD,<br>EntityTypeMETHOD,<br>EntityType.ANNOTAION_TYPE,<br>EntityType.LOCAL_VARIABLE,<br>EntityType.PARAMETER,<br>EntityType.TYPE_PARAMETER,<br>EntityType.TYPE_USE|
|@Retention | 이 어노테이션이 실제로 적용되고 유지되는 범위<br>RetentionPolicy.RUNTIME : 컴파일 이후에도 JVM에 의해 계속 참조, 리플렉션이나 로깅에 많이 사용<br>RetentionPolicy.CLASS : 컴파일러가 클래스를 참조할 때까지 유지<br>RetentionPolicy.SOURCE : 컴파일 전까지만 유지, 컴파일 이후에는 사라짐|
|@Constraint | TelephoneValidator와 매핑하는 작업 수행|
|message() | 유효성 검사가 실패할 경우 반환되는 메세지|
|groups() | 유효성 검사를 사용하는 그룹으로 설정|
|payload() | 사용자가 추가 정보를 위해 전달하는 값|


##### Exception & Error 

**Exception(예외)**
<div class="content-box">
어플리케이션이 정상적으로 동작하지 못하는 상황
입력값의 처리가 불가능하거나, 참조된 갑싱 잘못된 경우 등
개발자가 직접 처리할 수 있는 것이므로 미리 코드 설계를 통해 처리 가능!
</div>

**Error(에러)**
<div class="content-box">
주로 자바의 VM에서 발생<br>
예외와 달리 어플리케이션 코드에서 처리할 수 있는 것이 거의 없다고 보임<br>
OutOfMemory, StackOverFlow 등등 이 있음. <br>
발생 시점에 처리하는 것이 아니라, 미리 코드를 보며 문제가 발생하지 않도록 예방해서 원천적으로 차단하는 것이 중요함
</div>

**예외 클래스**
**Uncheked Exception** : RuntimeExcepion을 상속받는 Exception<br>
**Checked Exception** : 그 외 Exception들

|-|	Checked Exception|	Unchecked Exception|
|--|--|--|
|처리 여부|	예외 처리 필수	|명시적 처리를 강제하지 않음|
|확인 시점|	컴파일 단계	|런타임 단계|
|대표 예외 클래스	|IOException<br> SQLException|RuntimeException<br>NullPointerException<br>IllegalArgumentException<br>SystemException|

**예외 처리**

*1. 예외 복구 : 예외 상황을 파악해 문제를 해결*<br>
`try{...}catch(Exception e){...}` 사용

*2. 예외 처리 회피*<br>
예외가 발생한 메소드를 호출한 곳에서 처리하도록 전가<br>
`throw` 사용

*3. 예외 전환*<br>
1,2번의 혼합이라고 볼 수 있음. <br>
예외가 발생 시 예외의 종류에 따라 호출부로 예외 내용 전달하며 좀 더 적합한 예외 타입으로 전달, 또는 어플리케이션에서 예외 처리 단순화를 위해 wrapping해야하는 경우도 존재.<br>
```java
try{
	...
}catch(Exception e){
	throw ...
}
```

##### SpringBoot 예외 처리

<div class="content-box">
Web applciation에서는 예외가 발생하면 예외를 복구해서 정상적으로 처리하기 보다 요청을 보낸 클라이언트에 어떤 문제가 발생했는지 상황전달하는 경우가 많다.<br>
예외가 발생 시, 클라이언트에 오류 메세지를 전달하려면 각 레이어에 발생한 예외를 엔드포인트 레벨인 컨트롤러로 전달해야한다. 
</div>

**SpringBoot에서는** `@(Rest)ControllerAdvice` , `@ExceptionHandler`**를 통해 모든 컨트롤러의 예외를 처리**

basePackages속성을 통해 예외 관제 범위 지정 가능<br>
범위 설정이 없으면 전역 볌위에서 예외처리<br>
`@ExceptionHandler`를 통해 특정 컨트롤러 예외 처리(Local예외처리)


|예외 처리 우선순위||
|--|--|
|@ControllerAdvice클래스 내에 동일하게 handler 메서드가 선언 | 구체적인 예외 클래스가 지정된 쪽이 우선순위가 높음|
|@ControllerAdvice의 글로벌 예외 처리와 @Controller내의 컨트롤러 예외 처리에 동일한 타입의 예외 처리| 범위가 좁은 컨트롤러의 핸들러 메소드가 우선순위|

**커스텀 예외**
<div class="content-box">
자바에서 제공하는 표준 예외를 사용하면 어플리케이션의 모든 예외 상황 처리가능 하지만 네이밍에 개발자의 의도를 담을 수 있기때문에 이름만으로도 어느 정도 예외상황 예측 가능하고 예외 메세지를 상세히 적어야하는 번거로움을 덜음과 동시에 예외를 개발자가 직접 관리하기 수월해짐(주로 표준예외를 상속). 이와 더불어 커스텀 예외로 관리하면 의도하지 않았던 부분에서 발생한 예외는 개발자가 관리하는 예외처리 코드가 처리하지 않으므로 개발과정에서 커뮤니케이션 효율 높아짐.<br>
`@ControllerAdvice`, `@ExceptionHandler`를 사용하여 예외 상황을 한곳에서 처리함.
</div>

### 리딩 7주차

##### 액추에이터(Actuator)

<div class="content-box">
어플리케이션을 개발하고 운영하는 단계에 접어들면 정상적으로 동작하는지 모니터링하는 환경이 매우 중요해짐. 스프링 부트에서는 HTTP 엔드포인트나 JMX(Java Management Extensions)를 통해 어플리케이션을 모니터링하고 관리할 수 있는 기능을 제공한다.
</div>

**JMX(Java Management Extensions)**<br>
실행중인 어플리케이션의 상태를 모니터링하고 설정을 변경할 수 있게 해준다.<br> JMX를 통해 관리를 하려면 Managed Beans를 생성해야 한다.

**Actuator 종속성 추가**<br>
```java
  <dependency>
  	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
  </dependency>
```

##### 엔드포인트

<div class="content-box">
액추에이터의 엔드포인트는 애플리케이션의 모니터링을 사용하는 경로.<br>
스프링부트에서는 여러 내장 엔드포인트가 포함되어 있으며, 커스텀 엔드포인트를 추가 할 수도 있음. 액추에이터를 추가하면 기본적으로 엔드포인트 URL로 /actuator가 추가되며 이 뒤에 경로를 추가해 상세내역에 접근. 
</div>
<br>
만약 /actuator 경로가 아닌 다른 경로 사용하고 싶다면 아래와 같이 `application.properties` 파일에 작성하면됨.  

```java
management.endpoint.web.base-path=/custom-path
```

##### Actuator 기본 EndPoint 리스트

|||
|-|-|
|auditevents | 호출된 Audit 이벤트 정보 표시|
|beans | 애플리케이션에 있는 모든 스프링 빈 리스트 표시|
|caches | 사용 가능한 캐시 표시|
|conditions | 자동 구성 조건 내용 생성|
|configprops | @ConfigurationProperties의 속성 리스트 표시|
|env | 애플리케이션에서 사용할 수 있는 환경 속성 표시|
|health | 애플리케이션의 상태 정보 표시|
|httptrace | 가장 최근에 이뤄진 100건의 요청 기록 표시|
|info | 애플리케이션의 정보 표시|
|intergrationgraph | 스프링 통합 그래프 표시|
|loggers | 애플리케이션의 로거 구성 표시 및 수정|
|metrics | 애플리케이션의 메트릭 정보 표시|
|mappings | 모든 @RequestMapping의 매핑 정보 표시|
|quartz | Quartz 스케줄러 작업에 대한 정보 표시|
|scheduledtasks | 애플리케이션에서 예약된 작업 표시|
|sessions | 스프링 세션 저장소에서 사용자의 세션을 검색 및 삭제|
|shutdown | 애플리케이션 정상 종료|
|startup | 애플리케이션이 시작될 때 수집된 시작 단계 데이터 표시|
|threaddump | 스레드 덤프 수행|

**Spring MVC, spring WebFlux에서 추가로 사용할 수 있는 엔드포인트***

|||
|-|-|
|heapdump | 힘 덤프 파일 반환|
|jolokia | Jolokia가 클래스패스에 있을 때 HTTP를 통해 JMX 빈 표시|
|logfile | logging.file.name 또는 logginf.file.path 속성이 설정되어 있는 경우 로그 파일의 내용 반환|
|Prometheus | Prometheus 서버에서 스크랩할 수 있는 형식으로 메트릭 표시|

##### 엔드포인트 노출 설정 기본값

|ID|JMX	|WEB|
|-|-|-|
|auditevents	|O|	X|
|beans	|O|	X|
|caches	|O|	X|
|conditions	|O|	X|
|configprops	|O|	X|
|env	|O|	X|
|flyway	|O|	X|
|health	|O|	O|
|heapdump	|해당없음|	X|
|httptrace	|O|	X|
|info	|O|	X|
|intergrationgraph	|O|	X|
|jolokia	|해당없음|	X|
|logfile	|해당없음|	X|
|loggers	|O|	X|
|liquibase	|O|	X|
|metrics	|O|	X|
|mappings	|O|	X|
|Prometheus	|해당없음|해당없음|	
|quartz	|O	|X|
|scheduledtasks	|O|	X|
|sessions	|O|	X|
|shutdown	|O|	X|
|startup	|O|	X|
|threaddump	|O|	X|

##### Actuator 기능 

**애플리케이션 기본 정보(/info)**<br>
`application.properties` 파일에 `info.`로 시작하는 속성 값들을 정의
`http://localhost:8080/actuator/info` URL에 접근 시 정의한 속성 값을 바탕으로 생성된 정보가 JSON 형태로 제공됨.


**애플리케이션 상태(/health)**<br>
별도의 설정 없이 `http://localhost:8080/actuator/health` 에 접근하여 결과 확인<br>
주로 네트워크 계층에서 L4 레벨(Loadbalancing, 전송 계층에서의 부하 분산) 에서 상태를 확인하기 위해 사용


**빈 정보 확인(/beans)**<br>
스프링 컨테이너에 등록된 스프링 빈의 전체 목록을 표시하며, JSON 형식으로 빈 정보를 반환


**스프링 부트의 자동설정 내역 확인(/conditions)**<br>
크게 `positiveMatches` 와 `negativeMatches` 로 구분되며 자동설정의 `@Conditional` 에 따라 평가된 내용을 표시


**스프링 환경변수 정보(/env)**<br>
`application.properties` 파일, OS, JVM의 환경변수 표시


**로깅 레벨 확인(/loggers)**<br>
애플리케이션의 로깅 레벨 수준이 어떻게 설정되어 있는지 확인 가능
`GET` 메소드로 호출하면 확인이 가능하며 `POST` 메소드로 호출하면 로깅 레벨을 변경


**Actuator에 커스텀 기능 생성**
기존 기능에 내용을 추가하거나, 새로운 엔드포인트를 개발하는 방식으로 구현하며,`@Endpoint`가 명시된 클래스를 이용하여 구현


##### 서버 간 통신

**RestTemplate**
<div class="content-box">
<b>HTTP 통신 기능을 쉽게 사용하도록 설계된 Spring에서 제공되는 템플릿</b><br><br>
- RESTful 형식을 갖춘 템플릿<br>
- RESTful 원칙을 따르는 서비스를 편리하게 생성<br>
- 기본적으로 동기 방식으로 처리되며 비동기 방식으로 사용하고 싶을 경우 AsyncRestTemplate 사용<br>
- HTTP 프로토콜의 메서드에 맞는 여러 메서드 제공<br>
- HTTP 요청 후 JSON, XML, 문자열 등의 다양한 형식으로 응답 받음<br>
- 블로킹(blocking) I/O 기반의 동기 방식 사용<br>
- 다른 API 호출할 때 HTTP 헤더에 다양한 값 설정<br>
</div>

##### RestTemplate 동작 원리
||
|-|
|1. 애플리케이션에서 RestTemplate 선언하고 URI, HTTP 메서드, Body 설정|
|↓|
|2. 외부 API로 요청을 보내면 RestTemplate에서 HttpMessageConverter를 통해 RequestEntity를 요청 메시지로 변환|
|↓|
|3. RestTemplate에서는 변환된 요청 메시지를 ClientHttpRequestFactory를 통해 CLientHttpRequest로 가져온 후 외부 API로 요청 전달|
|↓|
|4. 외부에서 요청에 대한 응답을 받으면 RestTemplate은 ResponseErrorHandler로 오류를 확인하고 오류가 있다면 ClientHttpResponse에서 응답 데이터 처리|
|↓|
|5. 받은 응답 데이터가 정상적이라면 다시 한번 HttpMessageConverter를 거쳐 자바 객체로 변환해서 애플리케이션으로 반환|

##### RestTemplate의 대표 메서드
|메서드	|HTTP형태|설명|
|-|-|-|
|getForObject|GET|GET 형식으로 요청한 결과를 객체로 반환|
|getForEntity|GET|GET 형식으로 요청한 결과를 ResponseEntity 결과로 반환|
|postForLocation|POST|POST 형식으로 요청한 결과를 헤더에 저장된 URI로 반환|
|postForObject|	POST|POST 형식으로 요청한 결과를 객체로 반환|
|postForEntity|	POST|POST 형식으로 요청한 결과를 ResponseEntity 형식으로 반환|
|delete|DELETE|DELETE 형식으로 요청|
|put|PUT|PUT 형식으로 요청|
|patchForObject	|PATCH|	PATCH 형식으로 요청한 결과를 객체로 반환|
|optionsForAllow|OPTIONS|해당 URI에서 지원하는 HTTP 메서드 조회|
|exchange|ANY|HTTP 헤더에 임의로 추가 가능하며 어떤 메서드 형식에서도 사용 가능|
|execute|ANY|요청과 응답에 대한 콜백 수정|


##### RestTemplate 사용

**서버 프로젝트 생성**

서버 용도의 프로젝트를 생성하기 위해 톰캣 포트를 9090(또는 다른포트)으로 변경
프로젝트 `spring-boot-starter-web` 모듈만 의존성으로 추가
`Controller` `GET`과 `POST` 메서드 형식의 요청을 받기 위한 코드 추가

**RestTemplate 구현**<br>
```
             +---server1----+   +---server2---+
[client] <-> |  Controller  |   |             |
             |      ↕       |   |             |
		     | RestTemplate |<->| Controller  | 
			 +--------------+   +-------------+
```
<div class="content-box">
-클라이언트는 서버를 대상으로 요청을 보내고 응답을 받는 역할<br>
-SwaggerConfiguration 클래스 파일과 의존성 추가<br>
-GET 형식의 RestTemplate 작성하기<br>
&nbsp;스프링 프레임워크에서 제공하는 클래스인 UriConponentsBuilder를 사용하여<br>&nbsp;여러 파라미터를 연결해 URI 형식으로 만드는 기능 수행<br>
-POST 형식의 RestTemplate 작성하기<br>
&nbsp;파라미터에 값을 추가하는 작업과 RequestBody에 값을 담는 작업 수행<br>
&nbsp;postForEntity() 메서드를 사용할 경우에 파라미터는 데이터 객체
</div>

**RestTemplate 커스텀 설정**
<div class="content-box">
RestTemplate는 커넥션 풀을 지원하지 않으므로 매번 호출할 때마다 포트를 열어 커넥션을 생성하는데, 이를 방지하기 위해 커넥션 풀 기능을 활성화하여 재사용할 수 있게 해야하며, 가장 대표적인 방식은 아파치에서 제공하는 HttpClient로 대체해서 사용하는 방식
</div>

**HttpClient를 사용하기 위한 의존성 추가 코드**
```java
  <dependency>
      <groupId>org.apache.httpcomponents</groupId>
      <artifactId>httpclient</artifactId>
  </dependency>
RestTemplate 생성자 코드
  public RestTemplate(ClientHttpRequestFactory requestFactory) {
      this();
      setRequestFactory(requestFactory);
  }
```
**HttpCLient 생성**

1.`HttpClientBuilder.create()` 메서드 사용<br>
2.`HttpCLients.custom()` 메서드 사용<br>

생성한 HttpClient는 factory의 `setHttpClient()` 메서드를 통해 인자로 전달해서 설정하고, 설정된 factory 객체를 RestTemplate를 초기화하는 과정에서 인자로 전달

##### WebClient
<div class="content-box">
실제 운영환경의 어플리케이션은 정식 스프링 부트의 버전보다 낮은 경우가 많아 RestTemplate을 많이 사용하지만, 최신 버전에서는 RestTemplate이 지원 중단되어 WebClient를 사용할 것을 권고
</div>

**WebFlux 모듈 의존성 추가**
```java
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-webflux</artifactId>
  </dependency>
```

**WebClient 생성**

1.`create()` 메소드를 통한 생성<br>
2.`builder()` 를 이용한 생성<br>

WebClient는 우선 객체를 생성한 후 요청을 전달하는 방식으로 동작.<br> 
`builder()` 를 통해 `baseUrl()` 메소드에서 기본 URL을 설정하고 `defaultHeader()` 메소드로 헤더의 값을 설정.<br> 
일반적으로 WebClient 객체를 이용할 때 객체를 생성하고 재사용하는 방법으로 구현하는 것이 좋음. 한번 빌드된 WebClient 는 변경할 수 없으나 `Webclient.mutate()` 메소드를 사용하면 복제 가능.

