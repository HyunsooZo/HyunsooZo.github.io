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


**∙ Bean의 구현체가 여러개인 경우 주입 받는 방법**

<div class="content-box">
1. <span class="emphasis">@Primary</span> -> 해당 빈을 최우선으로 주입<br>

2. <span class="emphasis">@Qualifier("beanName")</span>  -> beanName으로 지정된 빈 주입<br>

3. <span class="emphasis">Set</span>  또는 <span class="emphasis">List</span>로 받기<br>

4. <span class="emphasis">Property</span>  이름을 <span class="emphasis">bean</span> 과 동일하게하기. <u><b>가장 흔히 사용</b></u>
</div>


**∙ Bean 의 Scope**

<div class="content-box">
<span class="emphasis">Singleton</span>:일반적인 방법, 하나만 만들어 계속 재활용
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


### Bean 생명주기 Callback

<div class="content-box">
DB Connection pull이나 Network socket처럼 Application시작 시점에 필요한 연결을 미리 해두고, Application 종료 시점에 연결을 모두 종료하는 작업을 진행하려면, 객체의 초기화와 종료 작업이 필요하다. <br>
</div>

**[Spring Bean 은 간단히 다음과 같은 LifeCycle을 가진다]**<br>
***" Object생성 - > DI "***
<div class="content-box">
Spring Bean은 객체를 생성하고, 의존관계주입이 다 끝난 다음에야 필요한 데이터를 사용할 수 있는 준비가 완료된다. <span class="emphasis">따라서 초기화 작업은 의존관계 주입이 모두 완료되고 난 다음에 호출되어야한다.</span> Spring은 의존관계 주입이 완료되면 Spring Bean에게 <span class="emphasis">CallBack Method를 통해 초기화 시점을 알려주는 다양한 기능을 제공한다,</span><br>
또한 <span class="emphasis">Spring Container가 종료되지 직전에 소멸 Callback</span>을 준다. <br>
따라서 안전하게 종료 작업을 진행 할 수 있다. 
</div>

**[Spring Bean 의 event LifeCycle]-(Singleton)**
***Spring Container 생성 -> Spring Bean 생성 -> 의존관계 주입 ->초기화 콜백 -> 사용 -> 소멸전 콜백 -> Spring 종료***

**++참고 : 객체의 생성과 초기화는 분리하도록 하자**<br>
**생성자는 필수정보(파라미터)를 받고, 메모리를 할당해 객체를 생성***하는 책임을 가진다.* **반면 초기화는 이렇게 생성된 값들을 활용해 외부커넥션을 연결하는 등 무거운 동작을 수행***한다.*<br>
*따라서 생성자 안에서 무거운 초기화 작업을 진행하기보다는 객체생성부분과 초기화 부분을 명확하게 나누는 것이 유지보수관점에서 좋다. 물론 초기화 작업이 내부 값들만 약간 변경하는 정도로 단순한 경우에는 생성자에서 한번에 다 처리하는게 나을 때도 있다!*


### Bean LifeCycle Callback (InterFace)

**Interface(InitializingBean, DisposableBean)**
```java
//InitializingBean , DisposableBran Interface
public class NetworkClient implements InitializingBean, DisposableBean {
    private String url;
    public NetworkClient() {
        System.out.println("생성자 호출, url" + url);
    }
    public void setUrl(String url){
        this.url = url;
    }

    public void connect(){
        System.out.println("connect :" + url);
    }

    public void call(String message){
        System.out.println("call : " + url + " message :" + message);
    }

    public void disconnect(){
        System.out.println("close " + url);
    }
    //afterPropertiesSet Method(by InitializingBean i/f)
    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("NetworkClient.afterPropertiesSet");;
        connect();
        call("초기화 연결메세지");
    }
    //destroy Method (by DisposableBean)
    @Override
    public void destroy() throws Exception {
        System.out.println("NetworkClient.destroy");
        disconnect();
    }
}
```
**출력결과**
```
생성자 호출, urlnull
NetworkClient.afterPropertiesSet
connect :http://hello.com
call : http://hello.com message :초기화 연결메세지
15:13:11.816 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@22a637e7, started on Sat May 13 15:13:11 KST 2023
NetworkClient.destroy
close http://hello.com
```

**초기화,소멸 Interface의 단점**
<div class="content-box">
이 인터페이스는 Spring 전용 인터페이스이므로 해당 코드가 Spring 전용 인터페이스에 의존하게 된다. <br>
초기화,소명 메서드의 이름을 변경할 수 없다 <br>
내가 코드를 고칠수 없는 외부라이브러리에 적용할 수 없다.
</div>

### Bean LifeCycle Callback (Method)

**임의 메서드 방식의 장점**
<div class="content-box">
메서드이름을 자유롭게 지정할 수 있다. <br>
Spring Bean이 Spring Code에 의존하지 않는다. <br>
코드가 아니라 설정정보를 사용하므로, 코드를 고칠 수 없는 외부라이브러리에도 초기화,종료메세지를 사용할 수 있다.
</div>

**사용예시(1)**
```java
public void init() {
        System.out.println("NetworkClient.init");
        connect();
        call("초기화 연결메세지");
    }

    public void close() {
        System.out.println("NetworkClient.close");
        disconnect();
    }
```

```java
public class BeanLifeCycleTrst {
    @Test
    public void lifeCycleTest() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
        NetworkClient client = ac.getBean(NetworkClient.class);
        ac.close();
    }

    @Configuration
    static class LifeCycleConfig{
        @Bean(initMethod = "init",destroyMethod = "close")
        public NetworkClient networkClient(){
            NetworkClient networkClient = new NetworkClient();
            networkClient.setUrl("http://hello.com");
            return networkClient;
        }
    }
}
```

**출력결과(1)**
```
생성자 호출, urlnull
NetworkClient.init
connect :http://hello.com
call : http://hello.com message :초기화 연결메세지
15:33:48.088 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@22a637e7, started on Sat May 13 15:33:47 KST 2023
NetworkClient.close
close http://hello.com
```

**설정정보 사용 특징**
<div class="content-box">
`@Bean` 의 `destroyMethod` 속성에는 특별한 기능이 있다. <br>
외부 라이브러리들은 대부분 `close` ,`shutdown`이라는 종료메서드 를 사용.<br>
`@Bean` 의 `destroyMethod`는 기본값이 `(inferred)`이다. <br>
이 기능은 `close`, `shutdown` 의 이름을 가진 메서드를 자동으로 호출해준다. 즉, inferred 뜻 그대로 추론하여 종료메서드들을 호출해준다. 따라서 Spring Bean으로 등록하면 굳이 다른 종료메서드를 적어줄 필요가 없다.<br>
사용을 원하지 않을 시에는 `destroyMethod=""`이렇게 공백을 넣어두면 된다. 
</div>


### `@Construct` , `@PreDestory` Annotation Callback**
