---
title: WebSocket
category: Projects
order: 1
---

### WebSocket

<div class="content-box">
웹소켓(WebSocket)은 양방향 통신을 지원하는 컴퓨터 네트워크 프로토콜이다. 웹소켓은 실시간으로 데이터를 주고받을 수 있는 기술로, 전통적인 HTTP 프로토콜의 단방향 통신 모델을 보완하기 위해 개발되었다.
</div>

|웹소켓의 주요 특징||
|--|--|
|양방향 통신|클라이언트와 서버 간에 양방향으로 데이터를 주고받을 수 있다|
|지속적인 연결| 한 번 연결된 웹소켓 연결은 유지되며, 연결을 계속해서 맺고 끊을 필요가 없다. 이는 일반적인 HTTP 요청과 달리 지속적인 연결을 통해 오버헤드를 줄일 수 있다.|
|낮은 지연과 오버헤드| 웹소켓은 HTTP보다 더 낮은 지연과 데이터 오버헤드를 가지며, 데이터를 빠르게 교환할 수 있다.|
|이벤트 기반| 서버나 클라이언트에서 발생하는 이벤트에 대한 실시간 처리를 지원.|
|프로토콜 확장 가능| 웹소켓은 확장 가능한 프로토콜이기 때문에 다양한 기능을 추가하여 사용할 수 있다.|


##### 의존성 추가

웨이팅 앱 팀프로젝트를 진행하며, 대기인원 수를 실시간으로 프론트에 전달하기 위해 웹소켓 사용을고려했고 아래와 같이 적용해보았다.

먼저 웹소켓 사용을 위해 다음의 의존성을 추가해주자.
```bash
	// WebSocket
	implementation 'org.springframework.boot:spring-boot-starter-websocket'
	implementation 'org.webjars:sockjs-client:1.0.2'
	implementation 'org.webjars:stomp-websocket:2.3.3'
```

### WebSocketConfig

##### 코드

```java
package com.bttf.queosk.config.websocketconfig;

import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        // 점주가 웨이팅 현황을 확인하는 페이지 엔드포인트
        registry.addEndpoint("/queue/restaurant")
                .setAllowedOriginPatterns("*")
                .withSockJS();

        // 점주가 웨이팅 현황을 확인하는 페이지 엔드포인트
        registry.addEndpoint("/queue/user")
                .setAllowedOriginPatterns("*")
                .withSockJS();
    }

    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.setApplicationDestinationPrefixes("/app");
        registry.enableSimpleBroker("/topic");
    }
}
```

##### 부연설명

웹소켓 통신이 필요한 페이지는 <고객의 대기현황 페이지>, <점주의 대기열 관리 페이지> <br> 총 두 페이지로 구성되었고 각 엔드포인트를 위와 같이 설정했다. <br>
Broker의 경우 구독 시 prefix로 사용할 값들을 넣게된다. 

### WebSocketService

##### 코드

```java
package com.bttf.queosk.service.queueservice;

import com.bttf.queosk.dto.queuedto.QueueRemaining;
import com.bttf.queosk.exception.CustomException;
import com.bttf.queosk.repository.QueueRedisRepository;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

import static com.bttf.queosk.exception.ErrorCode.FAILED_TO_FETCH_QUEUE;

@Slf4j
@RequiredArgsConstructor
@Service
public class QueueWaitingService {

    private final SimpMessagingTemplate messagingTemplate;
    private final QueueRedisRepository queueRedisRepository;

    // 전체 대기자 수 업데이트 (점주 화면용 웹소켓)
    @Transactional
    public void updateWaitingCount(Long restaurantId) {
        List<String> queueList = queueRedisRepository.findAll(String.valueOf(restaurantId));
        int waitingCount = queueList.size();

        QueueRemaining queueRemaining = QueueRemaining.builder()
                .waitingCount(waitingCount)
                .build();

        sendMessageToRestaurant(restaurantId, queueRemaining);
        log.info("총 대기 수 전달 완료 (식당 Id : " + restaurantId + " )");
    }

    // 고객 대기번호 업데이트(유저 대기화면용 웹소켓)
    @Transactional
    public void updateUserIndexes(Long restaurantId) {
        List<String> queueList = queueRedisRepository.findAll(String.valueOf(restaurantId));

        queueList.forEach(queueId -> {
            Integer index = queueList.indexOf(queueId) + 1; // 각 queueId의 인덱스(고객 대기번호) 구하기

            // QueueRemaining 객체 생성
            QueueRemaining queueRemaining = QueueRemaining.builder()
                    .waitingCount(index) // 업데이트된 대기번호 설정
                    .build();

            // 고객에게 업데이트된 대기번호 전달 (웹소켓)
            sendMessageToUser(restaurantId, Long.parseLong(queueId), queueRemaining);
        });

        log.info("고객 대기번호 업데이트완료  (식당 Id : " + restaurantId + " )");
    }

    private void sendMessageToUser(Long restaurantId, Long queueId, QueueRemaining queueRemaining) {
        try {
            String topic = "/topic/restaurant/" + restaurantId + "/queue/" + queueId;
            String queueRemainingJson = new ObjectMapper().writeValueAsString(queueRemaining);
            messagingTemplate.convertAndSend(topic, queueRemainingJson);
        } catch (JsonProcessingException e) {
            throw new CustomException(FAILED_TO_FETCH_QUEUE);
        }
    }

    private void sendMessageToRestaurant(Long restaurantId, QueueRemaining queueRemaining) {
        try {
            String topic = "/topic/restaurant/" + restaurantId;
            String queueRemainingJson = new ObjectMapper().writeValueAsString(queueRemaining);
            messagingTemplate.convertAndSend(topic, queueRemainingJson);
        } catch (JsonProcessingException e) {
            throw new CustomException(FAILED_TO_FETCH_QUEUE);
        }
    }
}
```

##### 부연설명
`SimpMessagingTemplate`은 Spring Framework에서 제공하는 클래스로, 웹소켓을 통해 메시지를 전송하는 데 사용되는 도구이다. Spring의 WebSocket 모듈을 사용하여 실시간 메시지 전달을 구현할 때 사용한다.

|SimpMessagingTemplate의 기능||
|---|---|
|메시지 전송|웹소켓을 통해 메시지를 클라이언트에게 전송할 수 있습니다. 특정 주제(topic)에 메시지를 보내거나, 특정 사용자나 세션에게 메시지를 보낼 수 있다.|
|구독 관리| 클라이언트가 특정 주제를 구독할 수 있도록 지원하며, 구독을 해지하는 기능도 제공한다.|
|간편한 통합|Spring의 다른 컴포넌트와 연동하여 사용하기 용이하다. 예를 들어, Spring Security와 함께 사용하여 인증된 사용자에게만 메시지를 전송하는 등의 작업이 가능하다.|

### 화면 내 Script

##### 코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Waiting App</title>

</head>
<body>
<h1>Queue 고객의 대기번호 와 고객 이전의 잔여 대기자 수 </h1>
<p>남은 대기인원: <span id="remainingCount">-1</span></p>
<p>고객 대기번호: <span id="waitingCount">0</span></p>
<script src="/webjars/sockjs-client/1.0.2/sockjs.min.js"></script>
<script src="/webjars/stomp-websocket/2.3.3/stomp.min.js"></script>
<script>
    var stompClient = null;
    var restaurantId = [[${restaurantId}]];
    var queueId = [[${queueId}]];
    var defaultCount = [[${defaultCount}]];

    // 페이지 로드시 디폴트 값으로 초기화
    window.addEventListener('load', function () {
        document.getElementById('remainingCount').textContent = defaultCount - 1;
        document.getElementById('waitingCount').textContent = defaultCount;
    });
    function connect(restaurantId) {
        var socket = new SockJS('/queue/user');
        stompClient = Stomp.over(socket);
        stompClient.connect({}, function (frame) {
            console.log('Connected: ' + frame);

            var topic = '/topic/restaurant/' + restaurantId + '/queue/' + queueId; // 매장 식별자를 포함한 토픽
            stompClient.subscribe(topic, function (message) {
                console.log('Received message:', message.body);
                updateWaitingCount(message.body);
            });
        });
    }

    function updateWaitingCount(messageBody) {
        var currentCountJson = JSON.parse(messageBody);
        var currentCount = currentCountJson.waitingCount;
        var remainingCount = currentCount - 1; // 현재 번호 - 1

        if (currentCount != null) {
            document.getElementById('waitingCount').textContent = currentCount;
            document.getElementById('remainingCount').textContent = remainingCount; // 남은 대기인원 업데이트
            console.log("Received count:", currentCount);
            console.log("Type of count:", typeof currentCount);
        }
    }

    // 페이지 로드시 초기 데이터 설정
    window.addEventListener('load', function () {
        connect(restaurantId);
    });
</script>
</body>
</html>
```
##### 부연설명
**connect 함수**: 웹소켓 서버와 연결을 설정하는 함수. <br> SockJS를 사용하여 웹소켓 연결을 생성하고, Stomp을 이용하여 해당 연결을 관리.<br> 서버에서 전송되는 메시지를 구독하고, 해당 메시지를 받을 때마다 updateWaitingCount 함수를 호출하여 대기번호 및 대기인원을 업데이트.
<br><br>
**updateWaitingCount 함수**: 서버로부터 전달된 메시지를 파싱하여 현재 대기인원 및 남은 대기인원을 계산하고, HTML 요소들을 업데이트하는 역할을 한다.
<br><br>
**페이지 로드 이벤트 리스너 (재사용)**: 페이지가 로드될 때, connect 함수를 호출하여 웹소켓 연결을 설정한다. 이로써 초기 데이터 설정과 실시간 업데이트가 가능해진다!


### 마무리

웹소켓의 동작원리와 적용방법을 알게되었다.<br> 
실제 프로젝트에 적용한 경험은 실시간 데이터 전송과 상호작용을 효과적으로 구현하는 강력한 도구로 사용자와의 실시간 소통을 강화하며, 웹소켓을 통한 실시간 업데이트는 서비스 품질을 높여주는 것 같다. <br>다음 프로젝트에서도 웹소켓을 적극 활용하여 더 효율적이고 현대적인 웹 애플리케이션을 만들어 보도록 해야겠다.



