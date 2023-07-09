---
title: Spring 북스터디 1~4주차 
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
|Client-서버 아키텍쳐|Rest서버는 API를 제공, Client는 사용자 정보를 관리. -> 서로 의존성 낮음 |

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
Delete는 데이터베이스 등의 저장소에 있는 리소스를 삭제할 때 사용.<br> 서버에서는 Client로부터 리소스를 식별할 수 있는 값을 받아 데이터베이스나 캐시에 있는 리소스를 조회하고 삭제. <br> 이 때 컨트롤러를 통해 값을 받는 단계에서 간단한 값만을 받기 때문에 Get 메서드와 같이 URI에 값을 넣어 요청을 받는 형식으로 구현.
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
Controller 클래스에 API를 구현. 여기에서 어떠한 요청을 받으면 데이터베이스는 서비스 클래스에서 핵심 기능을 제공.<br>데이터베이스와 밀접한 관련이 있는 객체는 엔티티 또는 DAO(Data Access Object)를 사용하고, Client와 가까워지는 서비스 등의 레이어에서는 데이터를 주고받을 때 DTO(Data Transfer Object) 객체 를 사용하는 것이 일반적.
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

