---
title: Spring Core Guide
category: ZeroBase BootCamp
order: 2
---
> 제로베이스 백엔드 부트캠프 - 북스터디 (마스터반)

### 1주차(5.15-5.22)

<div class="content-box">
01장: 스프링 부트란?<br>
02장: 개발에 앞서 알면 좋은 기초 지식<br>
03장: 개발 환경 구성
</div>

**[1]. 리딩 내용 점검**

`Ioc` : Inversion Of Control<br>
-제어의 역전. 즉 의존성 주입, 객체 생성 및 생명주기관리 등의 제어권한을 프레임워크에게 위임하는것. <br>

`DI` : Dependency Injection<br>
-Ioc 방법 중 하나로, 의존성 주입을 컨테이너가 생성한 객체를 주입받아 사용. (Annotation을 통해 주입하며 생성자 주입 방식이 권장됨)<br>

`AOP` : Aspect Oriented Programming<br>
-반복되는 부가기능 로직(로깅,트랜잭션,인증 등)을 모듈화 하여 여러 메서드 호출 전, 후 추가할 수 있다.<br> 
-AOP로 OOP를 더 잘 사용하도록 할 수 있다.<br>
-Spring은 proxy패턴을 이용하여 AOP를 제공한다.<br> 

|Spring Boot 의 장점||
|--|--|
|**의존성 관리 & 자동설정**| `@SpringBootApplication`은 크게 세가지 기능 <div class="content-box">`@ComponentScan`<br>`@EnableAutoConfiguration`<br>`@SpringBootConfiguration`</div> 을 합쳐놓은 것과 같고 자동으로 빈을 등록, 관리하여 어플리케이션에 반영함|
|**내장 WAS**| 내장 웹서버(WAS-tomcat)이 있어 별도 설정 불필요|
|**모니터링** | Spring Boot Actuator 툴을 사용해 시스템이 사용하는 주요요소를 모니터링 할 수 있음 |
<br><br>

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
RestAPI가 거의 표준처럼 사용된다하니 이후 토이프로젝트/개인프로젝트 진행 시에 이를 활용한 프로젝트를 진행해봐야겠다. 

이전에 진행했던 서울시 OpenAPI프로젝트를 스프링으로 한번 바꿔보고싶다. 과제와 진도에 시간이 부족하여 당장 해볼 순 없지만, 이후 시간이 남게되면 한번 해당 프로젝트를 스프링부트를 사용하여 다시 만들어보고싶다.
</div>

### 2주차(5.22-5.29)

<div class="content-box">
04장: 스프링 부트 애플리케이션 개발하기<br>
05장: API를 작성하는 다양한 방법
</div>

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