---
title: Bean Scope
category: Spring Framework
order: 7
---

### Bean Scope?

<div class="content-box">
Spring Bean은 기본적으로 Singleton Scope로 생성된다.<br> Scope는 번역 그대로 Bean이 존재할 수 있는 범위를 뜻한다. 
</div>

**Spring은 다음과 같은 다양한 Scope를 지원한다**
|Scope|Description|
|--|--|
|**Singleton**|기본스코프, 스프링 컨테이너의 시작-종료까지 유지되는 *가장 넓은* 범위의 스코프|
|**Prototype**|스프링컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 *매우 짧은* 범위의 스코프|
|**request**<br>(web)|web 요청이 들어오고 나갈 때까지 유지되는 스코프|
|**session**<br>(web)|Web 세션이 생성되고 종료될 때까지 유지되는 스코프|
|**application**<br>(web)|웹의 서블릿 컨텍스와 같은 범위로 유지되는 스코프|

**Compnent Scan 시**
```java
@Scope("prototype")
@Compnent
public class HelloBean{}
```

**수동 등록 시** 
```java
@Scope("prototype")
@Bean
PrototypeBean HelloBean(){
    return new HelloBean();
}
```

### Singleton Scope

<div class="content-box">
Singleton Scope Bean을 조회하면 Spring Container는 항상 같은 인스턴스의 Spring Bean을 반환한다. <br>

<b>1.</b> 싱글톤 스코프의 빈을 스프링 컨테이너에 요청<br>
<b>2.</b> 스프링 컨테이너는 본인이 관리하는 스프링 빈을 반환<br>
<b>3.</b> 이후 스프링 컨테이너에 같은 요청이 와도 같은 객체 인스턴스의 스프링 빈 반환
</div>


### Prototype Scope

<div class="content-box">
Prototype Scope 를 Spring Container에 조회하면 Spring Container는 항상 새로운 인스턴스를 생성하여 반환한다. <br>

<b>1.</b> 프로토 타입 스코프의 빈을 스프링 컨테이너에 요청<br>
<b>2.</b> 스프링 컨테이너는 이 시점에 프로토타입 빈을 생성하고, 필요한 의존관계에 주입하여 빈을 반환.<br>
<b>3.</b> 이후에 같은 ㅛ청이 와도 항상 새로운 프로토타입 빈을 생성해 반환
</div>

**핵심**<br>
<span class="emphasis">Spring Container</span>는 <span class="emphasis">prototype Bean 을 생성하고, 의존관계 주입, 초기화까지만</span>처리한다는 것이다. <br>
클라이언트에 Bean을 반환하고, 이후 Spring Container는 생성된 prototype Bean을 관리하지 않는다. 즉, prototype Bean을 관리할 책임은 prototype Bean을 받은 Client에 있다. <br>
그러므로 `@PreDestroy` 같은 종료 메서드가 호출되지 않는다. 



