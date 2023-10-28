---
title: Spring @Async
category: TIL
order: 2
---
### @Async

<div class="content-box">
비동기적으로 처리를 할 수 있게끔 스프링에서 제공하는 어노테이션<br>
해당 어노테이션을 붙이게 되면 각기 다른 쓰레드로 비동기적으로 실행됨.<br> 
이 어노테이션을 사용하기 위해서는 <span class="emphasis">@EnableAsync</span> 가 달려있는 configuration 클래스 필요.
</div>

##### @Async 주의사항 

- `@Async` **사용 시 주의사항**

    - **1.** **`public` 메서드일 것**

    - **2. 동일 클래스에서 호출하는** `Self-invocation` **이어서는 안됨**
        - 프록시를 사용하기 위해서 메서드는 public이어야 하고, Self-invocation를 사용하게되면 프록시를 무시하고 바로 메서드를 호출함
    - **3. 비동기 스레드에서 터진 Exception 처리**
        - 비동기 스레드에서 터진 Error는 메인까지 반환하지 못하므로 별도의 처리필요


- `SimpleAsyncTaskExecutor`

    - 기본적으로, 스프링은 비동기적으로 메서드를 실행하기 위해서 `SimpleAsyncTaskExecutor`를 사용
        - `SimpleAsyncTaskExecutor`는 요청이 오는대로 계속해서 쓰레드를 생성한다.
        - 이것을 어플리케이션 레벨 또는 각 메서드 레벨에서 `@override` 함으로써 `default`변경 가능

### 사용예제

##### AsyncConfig

```java
/**
 * 비동기 작업을 위한 Spring Async 구성
 */
@Configuration
@EnableAsync
public class AsyncConfig extends AsyncConfigurerSupport {

    /**
     * 비동기 작업에 사용할 Executor를 설정
     *
     * @return 사용할 Executor를 반환
     */
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(2); // 디폴트로 실행 대기 중인 Thread 수
        executor.setMaxPoolSize(10); // 동시에 동작하는 최대 Thread 수
        executor.setQueueCapacity(20); // CorePool이 초과될때 Queue에 저장했다가 꺼내서 실행. (20개까지 저장)
        executor.setThreadNamePrefix("async-"); // Spring에서 생성하는 Thread 접두사
        executor.initialize();
        return executor;
    }

    /**
     * 비동기 작업에서 발생한 예외를 처리하는 {@link AsyncUncaughtExceptionHandler}를 반환
     *
     * @return 비동기 작업에서 발생한 예외를 처리하는 {@link AsyncUncaughtExceptionHandler}
     */
    @Override
    public AsyncUncaughtExceptionHandler getAsyncUncaughtExceptionHandler() {
        return new CustomAsyncUncaughtExceptionHandler();
    }
}
```

주석 내용과 같이 기본 스레드 수 , 최대 스레드수 , 큐 가용범위,
스레드 이름 접두사 를 설정하고

예외처리를 위한 `AsyncUncaughtExceptionHandler` 를 넣어주었다. 

##### CustomAsyncUncaughtExceptionHandler

```java
/**
 * 비동기 작업에서 발생한 예외를 처리하는 {@link AsyncUncaughtExceptionHandler}
 * GlobalExceptionHandelr (@ControllerAdvice, @ExceptionHandler)는
 * 다른 스레드에서 발생한 예외를 캐치하지 못하므로 스레드 풀을 사용할 때 설정된 예외 처리기
 * (예: ThreadPoolTaskExecutor에 설정된 AsyncUncaughtExceptionHandler)를 사용하여 예외 처리
 */
@Slf4j
public class CustomAsyncUncaughtExceptionHandler implements AsyncUncaughtExceptionHandler {

    @Override
    public void handleUncaughtException(Throwable throwable, Method method, Object... params) {
        log.error("Async Exception - " + throwable.getMessage());
        log.error("Async Method - " + method.getName());
        Arrays.stream(params).forEach(param -> log.error("Parameter value - " + param));
    }
}
```

##### @Async 적용

```java
 @Async
    public CompletableFuture<String> sendEmail(String to,
                                               MailComponents subject,
                                               MailComponents text) {

        SimpleMailMessage mail = new SimpleMailMessage();
        String randomString = null;

        // 인증 메일 일 경우 인증 코드를 보내는 메일 생성
        if (subject.equals(VERIFICATION_SUBJECT)) {
            randomString = createUUID(6);
            createMail(mail, to, subject, text, randomString);
        }

        send(mail);
        log.info("email 전송 성공! 수신자 : " + to);

        return CompletableFuture.completedFuture(randomString);
    }
```

이제 이메일 메서드는 비동기로 실행되며 반환값은 <br>`CompletableFuture<String>` 으로 반환된다!<br>
이는 필요한 곳에서 `.get()` 하면 해당 스레드의 동작이 끝났을 때<br> 원하는 값을 반환한다.