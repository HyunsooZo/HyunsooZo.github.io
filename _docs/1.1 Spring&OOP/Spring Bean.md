---
title: Container & Bean
category: Spring Framework
order: 3
---

### Spring Container?

<div class="content-box">
<b>ApplicationContext</b>를 Spring Container라고 한다. <br>
<b>ApplicationContext</b>는 <b>Interfaced이다.</b> <br>
Spring Container는 <b>XMl 기반</b>으로 생성 또는 <b>Annotation기반 자바설정 클래스</b>로 생성.<br>
</div>

**∙ Spring Container는** `@Configuration` **Annotation을 가진 클래스를 구성정보로 사용한다.**

**∙** `@Bean`**이라고 적힌 Method를 모두 호출해서 반환된 객체를 Spring Container에 등록한다.(Spring Bean)**

**∙** `applicationContext.getBean("메서드명",클래스명.class)`**를 통해 SpringBean 을 찾을 수 있다.** 

**(implement)**  `applicationContext.getBean(클래스명.class)` **또는** `구현체.class`**를 통해서도 SpringBean 을 찾을 수 있다.** 

**∙ Spring Bean 조회 시 상속관계가 존재 할 경우에는 부모타입으로 조회시 자식타입도 모두 조회된다.**


### BeanFactory & ApplicationContext

<div class="content-box">
∙ Application Context는 BeanFactory의 모든 기능을 상속받는다.<br> 
∙ Application Context는 Bean의 모든 기능 + 부가기능을 제공한다.
</div>

|Application Context<br>의 부가기능|부가기능 설명|
|--|--|
|`MessageSource`|국제화기능(언어)|
|`EnvironmentCapable`|Local,dev,prod 구분하여 처리|
|`ApplicationEventPublisher`|이벤트 발행, 구독하는 모델 편리하게지원|
|`ResourceLoader`|파일,클래스패스,외부 등에서 리소스 편리하게 조회|

```
+-------------+
|Bean Factory |
|(interface)  |
+-------------+
      ↑ (implement) 
+-------------+
|App Context  |
|(interface)  |
+-------------+       
       ↑ (implement) 
+------------------+
|AnnotationConfig  |
|ApplicationContext| --> AppConfig.class
+------------------+    
```


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


### Resource 구현체 목록

**∙ URLResource**

java.net.URL 을 래핑한 버전. <br>
다양한 종류(ftp:,file:,http: 등의 prefix로 접근유형을 판단) 의 Resource에 접근 가능하지만 <u>기본적으로는 http(s)로 원격접근</u>

**∙ ClassPathResource**

classPath(소스코드를 빌드한 결과(기본적으로 target/classes폴더)) 하위의 리소스 접근 시 사용

**∙ FileSystemResource**

이름과 같이 file을 다루기 위한 리소스 구현체

**∙ ServletContextResource,InputStreamResource,ByteArrayResource**

servlet 어플리케이션 루트 하위 파일, InputStream,ByteArrayInput 스트림을 가져오기 위한 구현체


### BeanDefinition

**∙** `BeanDefiniation(Bean 설정 Meta Info)`은 "역할과 구현을 개념적으로 나눈 것" 즉, 추상화 이다. 

**∙** `AnnotationConfigApplicationContext`는 `AnnotatedBeanDefinitionReader`를 사용해서 `App.Config.class`를 읽고 `BeanDefinition`을 생성한다. 

|BeanDefinition||
|--|--|
|BeanClassName|생성할 빈의 클래스명(자바설정처럼 팩토리 역할의 빈을 사용하면 없음.)|
|factoryBeanName|팩토리 역할의 빈을 사용할 경우 이름|
|factoryMethodName|빈을 생성할 팩토리 메서드 지정|
|Scope|싱글톤(Default)|
|LazyInit|스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라<br>실제 빈을 사용할 때 까지 최대한 생성을 지연처리하는 지 여부|
|InitMethodName|빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드명|
|DestroyMethodName|빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드명|
|Constructor arguments<br>Properties|의존관계 주입에서 사용.<br>(자바설정처럼 팩토리 역할의 빈을 사용하면 없음)|


### 여러가지 Bean 설정방법

**∙ Bean의 구현체가 여러개인 경우 주입 받는 방법**

<div class="content-box">
1. <span class="emphasis">@Primary</span> -> 해당 빈을 최우선으로 주입<br>

2. <span class="emphasis">@Qualifier("beanName")</span>  -> beanName으로 지정된 빈 주입<br>

3. <span class="emphasis">Set</span>  또는 <span class="emphasis">List</span>로 받기<br>

4. </span>Property</span>  이름을 <span class="emphasis">bean</span> 과 동일하게하기. <u><b>가장 흔히 사용</b></u>
</div>


**∙ Bean 의 Scope**

<div class="content-box">
</span>Singleton</span>:일반적인 방법, 하나만 만들어 계속 재활용
</div>

**∙ Prototype: 매번 새로만드는 방법 (데이터 클리어 필요 시)**

<div class="content-box">
<span class="emphasis">Request</span>: 요청에 따라 계속 새로 만듦<br>
<span class="emphasis">Session</span>: 세션마다 계속 새로 만듦<br>
<span class="emphasis">WebSocket</span>: 양방향 실시간 통신
</div>

**∙Spring의 환경설정 : profile**

<div class="content-box">
∙ 현업에서는 환경을 다양하게 하여 해당 환경에서만 동작하는 Bean을 만드는 경우가있음.<br> 
∙ 클래스단위에 적용하거나 메서드 단위에 적용가능 
<div>

