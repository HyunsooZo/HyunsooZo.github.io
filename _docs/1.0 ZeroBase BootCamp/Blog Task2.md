---
title: Spring Core Guide
category: ZeroBase BootCamp
order: 2
---
> 제로베이스 백엔드 부트캠프 - 북스터디 (마스터반)

### 1주차(5.15-5.22)

01장: 스프링 부트란?<br>
02장: 개발에 앞서 알면 좋은 기초 지식<br>
03장: 개발 환경 구성

**[1]. 리딩 내용 점검**
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

|Spring Boot 의 장점||
|-|-|
|의존성 관리 & 자동설정| `@SpringBootApplication`은 자동으로 빈을 등록, 관리하여 어플리케이션에 반영함|
|내장 WAS| 내장 웹서버(WAS-tomcat)이 있어 별도 설정 불필요|
|모니터링| Spring Boot Actuator 툴을 사용해 시스템이 사용하는 주요요소를 모니터링 할 수 있음 |

|Rest의 특징||
|-|-|
|유니폼 인터페이스|Rest서버는 HTTP표준전송규약을 따르므로 프로그래밍언어와 상관없이 플랫폼/기술에 종속되지 않고 사용가능|
|무상태성|서버에 상태정보를 따로 보관/관리하지 않음. -> 자유도 높음|
|캐시가능성|HTTP의 캐싱기능 사용가능하고 사용함으로써 서버의 transaction부하 줄일 수 있음.|
|레이어 시스템|여러계층으로 구성될 수 있음.|
|클라이언트-서버 아키텍쳐|Rest서버는 API를 제공, 클라이언트는 사용자 정보를 관리. -> 서로 의존성 낮음 |

정리해보면, 이번 1,2,3 장에서 가장 중요한 내용은 <span class="emphasis">DI(의존성 주입)</span>이라고 생각한다. <br>
DI를 통해 객체는 직접 의존하는 객체를 생성하거나 관리하지 않고, 의존성을 가진 객체를 주입받아 사용하고, Spring에서는 이를 통해 코드의 결합도를 낮추고 유연성과 확장성을 향상시킬 수 있다.

**[2]. 응용 학습**
<div class="content-box">
이전에 진행했던 서울시 OpenAPI프로젝트를 스프링으로 한번 바꿔보고싶다. 과제와 진도에 시간이 부족하여 당장 해볼 순 없지만, 이후 시간이 남게되면 한번 시도해봐야겠다.
</div>

### 2주차(5.22-5.29)

<div class="content-box">
04장: 스프링 부트 애플리케이션 개발하기<br>
05장: API를 작성하는 다양한 방법

**메이븐(Maven)**
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

**API 작성방법**

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

### 3주차(5.29-6.5)

<div class="content-box">
06장: 데이터베이스 연동
</div>

### 4주차(6.5-6.12)

<div class="content-box">
8장: Spring Data JPA 활용
</div>

### 5주차(6.12-6.19)

<div class="content-box">
09장: 연관관계 매핑
</div>

### 6주차(6.19-6.26)

<div class="content-box">
10장: 유효성 검사와 예외 처리
</div>

### 7주차(6.26-7.3)

<div class="content-box">
11장: 액추에이터 활용하기 <br> 
12장: 서버 간 통신
</div>

### 8주차(7.3-7.10)

<div class="content-box">
13장: 서비스의 인증과 권한 부여
</div>