---
title: Spring Core Guide
category: ZeroBase BootCamp
order: 2
---
> 제로베이스 백엔드 부트캠프 - 북스터디 (마스터반)

### 리딩 1주차

01장: 스프링 부트란?<br>
02장: 개발에 앞서 알면 좋은 기초 지식<br>
03장: 개발 환경 구성

##### Spring Boot
<div class="content-box">
<b>Ioc : Inversion Of Control</b><br>
-제어의 역전. 즉 의존성 주입, 객체 생성 및 생명주기관리 등의 제어권한을 프레임워크에게 위임하는것. <br>

<b>DI : Dependency Injection</b><br>
-Ioc 방법 중 하나로, 의존성 주입을 컨테이너가 생성한 객체를 주입받아 사용. (Annotation을 통해 주입하며 생성자 주입 방식이 권장됨)<br>

<b>AOP : Aspect Oriented Programming</b><br>
-반복되는 부가기능 로직(로깅,트랜잭션,인증 등)을 모듈화 하여 여러 메서드 호출 전, 후 추가할 수 있다.<br> 
-AOP로 OOP를 더 잘 사용하도록 할 수 있다.<br>
-Spring은 proxy패턴을 이용하여 AOP를 제공한다.<br> 
</div>

##### Spring Boot 의 장점

|Spring Boot 의 장점||
|-|-|
|의존성 관리 & 자동설정| `@SpringBootApplication`은 자동으로 빈을 등록, 관리하여 어플리케이션에 반영함|
|내장 WAS| 내장 웹서버(WAS-tomcat)이 있어 별도 설정 불필요|
|모니터링| Spring Boot Actuator 툴을 사용해 시스템이 사용하는 주요요소를 모니터링 할 수 있음 |

##### Rest의 특징

|Rest의 특징||
|-|-|
|유니폼 인터페이스|Rest서버는 HTTP표준전송규약을 따르므로 프로그래밍언어와 상관없이 플랫폼/기술에 종속되지 않고 사용가능|
|무상태성|서버에 상태정보를 따로 보관/관리하지 않음. -> 자유도 높음|
|캐시가능성|HTTP의 캐싱기능 사용가능하고 사용함으로써 서버의 transaction부하 줄일 수 있음.|
|레이어 시스템|여러계층으로 구성될 수 있음.|
|클라이언트-서버 아키텍쳐|Rest서버는 API를 제공, 클라이언트는 사용자 정보를 관리. -> 서로 의존성 낮음 |

정리해보면, 이번 1,2,3 장에서 가장 중요한 내용은 <span class="emphasis">DI(의존성 주입)</span>이라고 생각한다. <br>
DI를 통해 객체는 직접 의존하는 객체를 생성하거나 관리하지 않고, 의존성을 가진 객체를 주입받아 사용하고, Spring에서는 이를 통해 코드의 결합도를 낮추고 유연성과 확장성을 향상시킬 수 있다.

### 리딩 2주차

04장: 스프링 부트 애플리케이션 개발하기<br>
05장: API를 작성하는 다양한 방법

##### 메이븐(Maven)
<div class="content-box">
아파치 메이븐은 자바 기반의 프로젝트를 빌드하고 관리하는 데 사용되는 도구<br>
pom.xml 파일에 필요한 라이브러리를 추가하면 해당 라이브러리에 필요한 라이브러리까지 함께 내려받아 관리함.
</div>

|메이븐의 대표 기능||
|--|--|
|프로젝트 관리 | 프로젝트 버전과 아티팩트를 관리|
|빌드 및 패키징 | 의존성 관리, 설정된 패키지 형식으로 빌드 수행|
|테스트 | 빌드를 수행하기 전 단위 테스트를 통해 작성된 애플리케이션 코드의 정상 동작 여부 확인|
|배포 | 빌드가 완료된 패키지를 원격 저장소에 배포|


**메이븐의 생명 주기**
<div class="content-box">
메이븐의 기능은 생명주기에 따라 관리/ 동작됨.
</div>

|**기본 생명 주기(Default Lifecycle)**||
|--|--|
|validate | 프로젝트를 빌드하는데 필요한 모든 정보를 사용할 수 있는지 검토|
|compile | 프로젝트의 소스코드를 컴파일|
|test | 단위 테스트 프레임워크를 사용해 테스트를 실행|
|pakage | 컴파일한 코드를 가져와서 JAR 등의 형식으로 패키징을 수행|
|verify | 패키지가 유효하며 일정 기준을 충족하는지 확인|
|install | 프로젝트를 사용하는 데 필요한 패키지를 로컬 저장소에 설치|
|deploy | 프로젝트를 통합 또는 릴리스 환경에서 다른 곳에 공유하기 위해 원격 |저장소에 패키지를 복사|


**클린 생명 주기(Clean LifeCycle)** : 이전 빌드가 생성한 모든 파일 제거
<br>

**사이트 생명 주기(Site LifeCycle)**<br>
`site` : 메이븐의 설정 파일 정보를 기반으로 프로젝트의 문서 사이트를 생성<br>
`site-deploy` : 생성된 사이트 문서를 웹 서버에 배포

##### API 작성방법

|**용어정리**||
|--|--|
|URL | 흔히 말하는 웹 주소, 리소스가 어디에 있는지 알려주기 위한 경로.|
|URI |  특정 리소스를 식별할 수 있는 식별자. 웹에서는 URL을 통해 리소스가 어느 서버에 위치해 있는지 알 수 있으며, 그 서버에 접근해서 리소스에 접근하기 위해서는 대부분 URI가 필요.|
|DTO |  Data Transfer Object로, 다른 레이어 간의 데이터 교환에 활용. DTO는 데이터를 교환하는 용도로만 이용되기에 별도의 로직이 포함되지 않음.|
|VO | 데이터 그 자체로 의미가 있는 객체. 읽기 전용이고, 값을 변경할 수 없게 만들어 데이터의 신뢰성을 유지해야 할 필요있음.|
|Swagger | API 명세를 관리하기 위해 도움을 주는 오픈소스 프로젝트|
|Logging|애플리케이션이 동장하는 동안 시스템의 상태나 동작 정보를 시간순으로 기록하는 것.|
|Logback|스프링 부트의 spirng-boot-starter-web 라이브러리 내부에 내장돼있어 별도의 의존성을 추가필요 x|

> GET API 만들기

```java
@GetMapping(value="/hello") // 4.3 이후 버전
public String getHello() {
	return "Hello World";
}
```
`@PathVariable`을 활용한 GET 메서드
```java
@GetMapping(value = "/variable1/{variable}")
public String getVariable1(@PathVariable String variable) {
	return variable;
}
```

`@RequestParam`을 활용한 GET 메서드
```java
// http://localhost:8080/api/v1/get-api/request1?name=value1&email=value2&organization=value3
@GetMapping(value = "/request1")
public String getRequestParam1(
	@RequestParam String name,
	@RequestParam String email,
	@RequestParam String organization) {
    return name + " " + email + " " + organization;
}
```

> POST API 만들기

`@RequestBody`로 `MemberDto`를 설정해두면 `MemberDto`의 멤버 변수를 요청 메시지의 키와 매핑에 값을 가져온다.
```java
// http://localhost:8080/api/v1/post-api/member2
@PostMapping(value = "/member2")
public String postMemberDto(@RequestBody MemberDto memberDto) {
	return memberDto.toString();
}
```

> PUT API 만들기
<div class="content-box">
Web Application Server 를 통해 저장소(DB 등..)에 존재하는 리소스 값을 업데이트 하는 데 사용. <br> POST와 비교하면 요청을 받아 실제 데이터베이스가 반영하는 과정(서비스 로직)에서 차이가 있지만 컨트롤러 클래스 구현 방법은 POST와 거의 동일. 리소스를 서버에 전달하기 위해 HTTP Body 활용
</div>

```java
// http://localhost:8080/api/v1/put-api/member1
@PutMapping(value = "/member1")
public String putMemberDto(@RequestBody MemberDto memberDto) { // A - MemberDto 클래스에서 오버라이딩한 .toString() 메서드를 리턴
public MemberDto putMemberDto(@RequestBody MemberDto memberDto) { // B - memberDto 인스턴스 자체를 리턴
	return memberDto.toString(); // A
    return memberDto;			 // B
}
```
Response
```java
//A 응답(context-type: text/plain;charset=UTF-8)
//-> body에 toString() 메서드 결괏값이 text/plain 형태의 content-type으로 응답
MemberDTO{name='Flature', email='thinkground.flature@gmail.com', organization='Around Hub Studio'}

//B 응답(context-type: application/json;charset=UTF-8)
//-> body에 json 형태의 결괏값이 application/json 형태의 content-type으로 응답
{
	name : "Flature",
    email : "thinkground.flature@gmail.com",
    organization : "Around Hub Studio"
}
```
> DELETE API 만들기
<div class = "content-box">
Delete는 데이터베이스 등의 저장소에 있는 리소스를 삭제할 때 사용.<br> 서버에서는 클라이언트로부터 리소스를 식별할 수 있는 값을 받아 데이터베이스나 캐시에 있는 리소스를 조회하고 삭제. <br> 이 때 컨트롤러를 통해 값을 받는 단계에서 간단한 값만을 받기 때문에 Get 메서드와 같이 URI에 값을 넣어 요청을 받는 형식으로 구현.
</div>

```java
@RestController  
@RequestMapping("/api/v1/delete-api")
public class DeleteController {
      
}
```
`@PathVariable`을 사용한 DELETE 메서드 구현
```java
//http://localhost:8080/api/vi/delete-api/{String 값}
@DeleteMapping ( value = "/{vatiable}" )
public String DeleteVariable(@PathVariable String variable){
    return variable;
}
```
`@RequestParam`을 사용한 DELETE 메서드 구현
```java
//http://localhost:8080/api/vi/delete-api/request1?email=value
@DeleteMapping ( value = "/request1" )
public String getRequestParam1(@RequestParam String email){
    return "e-mail : " + email;
}
```

> 참고

`ResponseEntity`를 사용하면 원하는 HttpStatus와 body를 간단하게 결합하여 응답을 만들 수 있음.<br>

```java
// http://localhost:8080/api/v1/put-api/member3
@PutMapping(value = "/member3")
public ResponseEntity<MemberDto> putMemberDto(@RequestBody MemberDto memberDto) {
	return ResponseEntity
    	.status(HttpStatus.ACCEPTED) // 응답코드 202
    	.body(memberDto);
}
```

### 리딩 3주차

06장: 데이터베이스 연동<br>

##### ORM
<div class="content-box">
ORM(Object Relational Mapping): 
자바와 같은 객체지향 언어에서의 객체와 RDB(Relational Database, 관계형 데이터베이스)의 테이블을 자동으로 매핑하는 방법.<br>
ORM을 이용하면 쿼리문을 작성이 아닌 <u>코드로 데이터를 조작 가능</u>.
</div>

|ORM의 장점|ORM의 단점|
|--|-|
|쿼리를 객체지향적으로 조작가능|복잡한 쿼리의 경우 구현하기 어려우며 성능 문제가 발생 가능성 존재.|
|코드가 간결해지고 재사용/유지보수 용이|객체의 관점과 데이터베이스의 관계의 관점의 차이로 혼동이 발생 가능성있음.|
|데이터베이스에 대한 종속성이 줄어든다||


##### JPA
JPA(Java Persistence API): 자바의 ORM 기술 표준으로 채택된 InterFace. ORM=개념, JPA=구체적인 명세 와 같이 볼 수 있음. <br>

JPA는 내부적으로 JDBC를 사용, 개발자가 JDBC를 활용하여 구현하면 SQL에 의존하게 되어 효율이 떨어진다.<br> JPA는 이 문제점을 보완하여 개발자 대신 요구하는 기능에 맞는 SQL을 생성/ 데이터베이스 조작하여 객체와 데이터베이스를 연결.<br> 이를 기반으로 구현한 것 중 가장 많이 사용되는 것이 하이버네이트(Hibernate)이고, 스프링 부트에서는 이 하이버네이트의 기능을 더욱 편하게 사용하도록 구현된 Spring Data JPA를 활용할 수 있음.
<br>

**Method 생성 규칙**

<div class="content-box">
기본적인 메소드는 생성이 되어있지만 규칙에 따라 커스텀 메서드도 생성가능.<br>이를 통해 CRUD(Create, Read, Update, Delete)구현 가능.
</div>

|Methods||
|--|--|
|findBy|일치하는 값을 가지는 컬럼조회<br>SQL의 where 역할|
|StartsWith/StartingWith<br>EndsWith/EndingWith | 특정 키워드로 시작하거나 끝나는 조건 설정|
|Before/After| 시간을 기준으로 검색|
|Between| 두 값 사이의 데이터를 검색|

**JPA에서 업데이트 시**

조회, 추가, 삭제 의 메소드와는 조금 다름.<br> 
update 라는 키워드를 따로 사용하지 않으며, 값을 find() 를 통해 가져오면 객체가 영속성 Context에 추가됨.<br> 
컨텍스트가 유지되는 상황에서 코드에서 객체의 값을 변경하고 다시 save() 를 실행하면 변경 감지를 수행하여 값을 변경함.

##### Entity

<div class="content-box">
데이터베이스의 테이블에 대응하는 클래스.<br> 
데이터베이스의 테이블과 컬럼을 정의할 수 있으며, 어노테이션을 사용하여 다양한 관계 정의 가능.
</div>

|관련 Annotations||
|--|--|
|@Entity| 해당 클래스가 엔티티임을 명시|
|@Table|테이블 이름과 클래스 이름을 다르게 지정해야 할 때 사용.<br> 명시하지 않으면 둘의 이름이 같다는 의미이며, 다른 이름을 쓰려면 @Table(name= 이름)와 같이 별도 명시 필요.|
|@Id|테이블의 기본 값으로 사용되며, 모든 엔티티는 @Id 어노테이션이 필요|
|@GeneratedValue| 일반적으로 @Id 와 함께 사용,자동생식 방식 설정.<br> 흔히 데이터베이스에 위임하는 IDENTITY 를 많이 사용하며, 이는 DB의 AUTO_INCREMENT 를 사용해 기본값을 생성.|
|@Column|엔티티 클래스의 멤버는 자동으로 컬럼으로 매핑되지만, 몇 가지 설정이 필요한 경우 사용.(컬럼의 이름, nullable 여부, 데이터 길이, 유니크 컬럼 등)|

##### Controller 

<div class="content-box"> 
Controller 클래스에 API를 구현. 여기에서 어떠한 요청을 받으면 데이터베이스는 서비스 클래스에서 핵심 기능을 제공.<br>데이터베이스와 밀접한 관련이 있는 객체는 엔티티 또는 DAO(Data Access Object)를 사용하고, 클라이언트와 가까워지는 서비스 등의 레이어에서는 데이터를 주고받을 때 DTO(Data Transfer Object) 객체 를 사용하는 것이 일반적.
</div>

##### Service

<div class="content-box">
Service는 일반적으로 비즈니스 로직을 구현하는 계층. <br>Controller와 DAO(Data Access Object) 사이에서 데이터 처리를 담당하며, 핵심 비즈니스 로직을 실행.<br>             

서비스 계층은 컨트롤러(Controller)에서 받은 요청을 처리하고, 필요한 데이터를 데이터베이스에서 가져와 가공한 후 결과를 반환.<br> 서비스 클래스는 엔티티(Entity) 또는 DAO와 밀접하게 상호작용하며, 엔티티나 DAO의 메서드를 호출하여 데이터를 읽거나 쓸 수 있음.
</div>

##### Repository

Repository : Spring Data JPA가 제공하는 인터페이스<br>

<div class="content-box">
JpaRepository를 상속받는 인터페이스를 생성, 엔티티가 생성한 데이터베이스에 접근하는 데 사용.
</div>

```java
// 기본값(@id) 타입으로 지정하여 Repository I/F 생성 후 
// JpaRepository 상속
public interface PersonRepository extends JpaRepository<Person, Long> {
}
```

### 리딩 4주차

08장: Spring Data JPA 활용<br>

##### JPA

**JPQL**

<div class="content-box">
JPA에서 사용되는 쿼리문. <br>
SQL과 비슷한 문법을 사용하나 SQL에서는 column의 이름을 사용,PQL은 매핑된 Entity의 이름과 field의 이름 사용.
</div>

**Query Method**<br>
<div class="content-box">
리포지토리는 JpaRepository를 상속받는 것만으로도 기본적인 CRUD 메서드를 제공하지만 별도의 메서드를 정의해서 사용하는 경우가 많다.
</div>

**Query Method 생성**<br>
<div class="content-box">
쿼리 메서드는 동작을 결정하는 주제(Subject)와 서술어(Predicate)로 구분.<br> By로 서술어의 시작을 나타내어 구분자역할을 함.<br> 
서술어 부분은 검색 및 정렬 조건을 지정하는 영역이며, 기본적으로 엔티티의 속성으로 정의할 수 있고, AND나 OR를 사용해 조건을 확장할 수도 있다.
</div>

```java
// (리턴타입) + (주제 + 서술어)
List<Person> findByLastnamdAndEmail(String lastName, String email)
```

**Query Method의 주제 키워드**

**조회**<br>
리턴 타입 = Collection이나 Stream에 속한 하위 타입 설정
```java
find...By
read...By
get...By
query...By
search...By
stream...By
```

**특정 데이터가 존재하는지 확인**
```java
exists...By 
```

**쿼리 결과로 나온 레코드 수 리턴**
```java
count...By
```

**삭제**<br>
리턴 타입이 없거나 삭제한 횟수를 리턴
```java
delete...By
remove...By
```

**조회된 결괏값의 개수 제한**<br>
주제와 By 사이에 위치. 일반적으로 한 번의 동작으로 여러 건을 조회할 때 사용, 단 건으로 조회하기 위해서는 <number>를 생략하면 됨.
```java
...First<number>...
...Top<number>...
```

##### 정렬과 페이징

**정렬**<br>
OrderBy...Asc, OrderBy...Desc
정렬 구문은 And나 Or 키워드를 사용하지 않고 우선순위를 기준으로 차례대로 작성.

```java
private Sort getSort() {
	return Sort.by(
    	Order.asc("price"),
        Order.desc("stock")
    );
}
```

**페이징 처리**<br>
데이터베이스의 레코드를 개수로 나눠 페이지를 구분하는 것

JPA에서는 페이징 처리를 위해 Page와 Pageable을 사용.

```java
// 페이징 처리
Page<Product> findByName(String name, Pageable pageable);

// 페이징 쿼리 메서드 호출
Page<Product> productPage = productRepository.findByName("펜", PageRequest.of(0, 2));
```

**of 메서드**<br>
`of(int page, int size)`: 페이지 번호(0부터 시작), 페이지당 데이터 개수를 매개변수로 받으며 데이터를 정렬하지 않는다.
`of(int page, int size, Sort)`: 페이지 번호, 페이지당 데이터 개수, 정렬을 매개변수로 받으며 sort에 의해 정렬된다.
`of(int page, int size, Direction, String... properties)` : 페이지 번호, 페이지당 데이터 개수, 정렬 방향, 속성(칼럼)을 매개변수로 받으며,`Sort.by(direction, properties`)에 의해 정렬됨.<br>

Page 객체를 그대로 출력하면 해당 객체의 값을 보여주지 않고 몇 번째 페이지에 해당하는지만 확인할 수 있음.
각 페이지를 구성하는 세부적인 값을 보려면 아래 코드와 같이 작성해야 힘.

```java
Page<Product> ProductPage = productRepository.findByName("펜", PageRequest.of(0, 2));
System.out.println(productPage.getContent());
```

##### QueryDSL

 **QueryDSL** 

<div class="content-box">
 정적 타입을 이용해 SQL과 같은 쿼리를 생성할 수 있도록 지원하는 프레임워크
 </div>

문자열이나  XML 파일을 통해 쿼리를 작성해는 대신 QueryDSL이 제공하는 Fluent API를 활용해 쿼리를 생성할 수 있다.

|QueryDSL 장점|
|--|
|IDE가 제공하는 코드 자동 완성 기능 사용가능.|
|문법검사를 통해 문법 오류를 발생시키지 않음.|
|동적으로 쿼리생성 가능.|
|코드로 작성하므로 가독성/생산성 상승.|
|도메인 타입과 프로퍼티를 안전하게 참조 가능.|

<br>

**JPA Auditing 적용**

<div class="content-box">
생성 주체, 생성 일자, 변경 주체, 변경 일자와 같은 필드들은 매번 엔티티를 생성하거나 변경할 때마다 값을 주입함. <br>
이 값들을 자동으로 넣어주는 기능이 JPA Auditing!!!.
</div>

별도의 Configuration 클래스를 생성하여 사용하는 것을 권장.

|Annotation/Class||
|--|--|
|BaseEntity | 각 Entity에 공통으로 들어가게 되는 컬럼을 하나의 클래스로 만듦.|
|@EnableJpaAuditing| 어노테이션을 추가|
|@MappedSuperclass| JPA의 엔티티 클래스가 상속받을 경우 자식 클래스에게 매핑 정보를 전달|
|@EntityListeners | 엔티티를 데이터베이스에 적용하기 전후로 콜백을 요청할 수 있게 함|
|AuditingEntityListener |엔티티의 Auditing 정보를 주입하는 JPA 엔티티 리스너 클래스|
|@CreatedDate | 데이터 생성 날짜를 자동 주입.|
|@LastModifiedDate | 데이터 수정 날짜를 자동 주입.|
|@CreatedBy<br>@ModifiedBy| 누가 엔티티를 생성했고 수정했는지 자동으로 값을 주입하는 기능, 이 기능을 사용하려면 AuditorAware를 스프링 빈으로 등록해야 함.|

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
