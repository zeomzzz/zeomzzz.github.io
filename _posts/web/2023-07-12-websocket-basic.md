---
title:  "WebSocket이란?"
excerpt: ""

categories:
  - Web
tags:
  - [Web, WebSocket]

toc: true
toc_sticky: true
 
date: 2023-07-12
last_modified_at: 2023-07-12
---

<br>


<br>

# **Web Socket**

<br>

## **웹 소켓이란?**

<br>

- 전 이중 통신 채널을 통해 실시간 성을 보장하는 서비스

ex. 게임, 채팅, 실시간 주식 거래 사이트 등등

<br>

>💡 **HTTP vs. Web Socket**
>
>- HTTP에도 실시간성을 보장하는 기법이 존재함 : Polling, Long Polling, Streaming
>    
>    ex. 서버 쪽으로 클라가 지속적으로 요청 보냄, 한 번 요청 보내고 연결 끊지 않고 계속 요청 보냄
>    
>- 차이점 : 수립된 커넥션을 어떻게 처리하느냐
>
>| HTTP | Web Socket |
>| --- | --- |
>| 비연결성 (클라가 연결해달라고 해야 연결. 끝나면 끊음) | 연결 지향 (계속 연결 → Handshake 안해도 됨) |
>| 매번 연결을 맺고 끊는 과정의 비용 | 한 번 연결 맺은 뒤 유지 |
>| (요청-응답) 구조 : 쌍을 이룸 | 양방향 통신 |

- 지원 환경 ([caniuse](https://caniuse.com/) 참고)
    
    ![]({{ site.url }}{{ site.baseurl }}/assets/images/web/websocket-basic/01.png ){: .align-center}
    
    - Web Socket 미지원 환경 : SockJs, Socket.io 이용

<br>

## **Spring에서 WebSocket 사용하기**

<br>

- 의존성 추가
    - `org.json.JSONObject` : 웹 소켓의 데이터 통신은 내부적으로 JSON을 사용
- 구조
    
    
    | annotation | class | method |  |  |
    | --- | --- | --- | --- | --- |
    | @Configuration <br> @EnableWebSocket | WebSocketConfig implements WebSocketConfigurer | registerWebSocketHandlers | addHandler | handler(직접 구현)와 handshake 할 주소 설정 |
    |  |  |  | setAllowedOrigins | CORS 설정 <br> : 기본 정책은 same origin만 허용 |
    |  |  |  | withSockJS | 웹소켓을 미지원 브라우저 환경에서도 비슷한 경험을 제공하기 위해 SockJS 설정 |
    |  | SocketTextHandler extends TextWebSocketHandler | afterConnectionEstablished |  | 커넥션이 맺어질 때, Collection에 웹소켓 세션을 추가 |
    |  |  | handleTextMessage |  |  |
    |  |  | afterConnectionClosed |  | 커넥션이 끊어질 때, Collection의 웹소켓 세션을 제거 |
- 코드
    - WebSocketConfig.java
        
        ```java
        import org.springframework.context.annotation.Configuration;
        import org.springframework.web.socket.config.annotation.EnableWebSocket;
        import org.springframework.web.socket.config.annotation.WebSocketConfigurer;
        import org.springframework.web.socket.config.annotation.WebSocketHandlerRegistry;
        
        @Configuration
        @EnableWebSocket
        public class WebSocketConfig implements WebSocketConfigurer {
        
        	@Override
        	public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        		registry.addHandler(new SocketTextHandler(), "/user")
        			.setAllowedOrigins("*")
        			.withSockJS();
        	}
        
        }
        ```
        
        | 구분 | 상세 |
        | --- | --- |
        | addHandler | - SocketTestHandler : 직접 구현한 웹소켓 핸들러 <br> - “/user” : handshake 할 주소 |
        | setAllowedOrigins | - CORS 설정 (기본 : same origin) |
        | withSockJS() | - SockJS 사용 설정 |
    - SocketTextHandler.java
        
        >💡 WebSocket 프로토콜은 기본적으로 text와 binary 타입 지원
        >-> 스프링이 제공하는 기본 클래스인 TextWebSocketHandler 또는 BinaryWebSocketHandler를 상속하고 구현
        
        <br>
        
        >💡 **WebSocketSession**
        >
        >- WebSocketSession != HttpSession
        >- WebSocket이 연결될 때 생기는 연결 정보를 담은 객체
        >- handler에서 WebSocket 통신에 대한 처리를 하기 위해 세션들을 colection으로 담아서 관리하는 경우가 많음 ex. Set
        
        <br>
        
        ```java
        import java.util.Set;
        import java.util.concurrent.ConcurrentHashMap;
        
        import org.json.JSONObject;
        import org.springframework.web.socket.CloseStatus;
        import org.springframework.web.socket.TextMessage;
        import org.springframework.web.socket.WebSocketSession;
        import org.springframework.web.socket.handler.TextWebSocketHandler;
        
        public class SocketTextHandler extends TextWebSocketHandler {
        	
        	private final Set<WebSocketSession> sessions = ConcurrentHashMap.newKeySet();
        
        	@Override
        	public void afterConnectionEstablished(WebSocketSession session) { // 커넥션이 맺어질 때, Collection에 웹소켓 세션을 추가  
        		sessions.add(session);
        	}
        
        	@Override
        	protected void handleTextMessage(WebSocketSession session, TextMessage message) throws Exception {
        		String payload = message.getPayload();
        		JSONObject jsonObject = new JSONObject(payload);
        		for (WebSocketSession s : sessions) {
        			s.sendMessage(new TextMessage("Hi " + jsonObject.get("user") + "! How may I help you?"));
        		}
        	}
        	
        	@Override
        	public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception { // 커넥션이 끊어질 때 제거
        		sessions.remove(session);
        	}
        	
        }
        ```
        
        - 이와 같이 처리하면, 모든 클라이언트에게 메시지를 보내는 등의 처리가 가능

<br>
<br>

# **Spring Messaging**

<br>

## **STOMP**

<br>

- Simple Text Oriented Messaging Protocol
- 메시지 브로커를 활용하여 쉽게 메시지를 주고 받을 수 있는 프로토콜
    - 메시지 브로커 : 발신자의 메시지를 받아와서 수신자들에게 메시지를 전달하는 어떤 것
    - Pub-Sub(발행-구독) : 발신자가 메시지를 발행하면 수신자가 그것을 수신하는 메시징 패러다임
- 웹소켓 위에 얹어 함께 사용할 수 있는 하위(서브) 프로토콜
- 프레임 단위의 프로토콜
    - 프레임 : 커맨드/헤더/바디의 형식
- STOMP는 웹소켓만을 위해 만들어진 프로토콜은 아님. 하지만
    - 웹소켓과 같은 양방향 통신 프로토콜에서 함께 사용할 수 있음
    - 스프링이 웹소켓 위해 STOMP 얹어서 사용하는 방법을 지원

<br>
<br>

## **왜 STOMP를 사용할까?**

<br>

- 웹소켓은 텍스트와 바이너리 타입의 메시지를 양방향으로 주고 받을 수 있는 프로토콜이지만 메시지를 어떤 형식으로 주고 받을지는 정해진 것이 없음
- 웹소켓만 사용해도 간단한 애플리케이션은 충분히 구현이 가능하지만, 프로젝트가 커지면 클라이언트와 서버가 메시지 주고 받는 형식, 메시지 타입, 메시지의 본문과 설정정보와 같은 데이터의 구분 등을 따로 정의를 해야하고 이를 파싱하는 로직도 구현해야 함
    
    -> STOMP 사용하면 해결!
    

ex. 웹소켓 : 메시지 자체만 송수신

STOMP : 커멘드, 헤더, 바디의 구조로 메시지를 송수신

<br>
<br>

## **통신 흐름**

<br>

- 스프링이 STOMP 이용할 때의 통신 흐름
    
    ![]({{ site.url }}{{ site.baseurl }}/assets/images/web/websocket-basic/02.png ){: .align-center}
    
    - 가정
        - 구독자에게 메시지를 보내고 싶어하는 발신자와 받고 싶은 구독자
        - 구독자는 /topic이라는 경로를 구독
    - 발신자는 바로 /topic을 바로 destination 헤더로 넣어서 메시지 송신
        
        또는 서버내에서의 처리, 가공이 필요하면 /app 이라는 주소로 메시지를 송신
        
    - 가공되거나 처리된 메시지는 /topic이라는 경로 담아서 다시 전달하면 MessageBroker에게 전달
    - MessageBroker는 메시지를 /topic을 구독하고 있는 구독자에게 최종적으로 전달
    - 서버단에서 어떤 처리나 메시지 가공할 필요가 없다면 발신자가 바로 메시지 브로커 통해서 구독자에게 보내는 것도 가능

<br>
<br>

## **구현**

<br>

- 구조
    
    
    | annotation | class | method |  |
    | --- | --- | --- | --- |
    | @Configuration <br> @EnableWebSocketMessageBroker | WebSocketBrokerConfig implements WebSocketMessageBrokerConfigurer | configureMessageBroker | STOMP에서 사용하는 메시지브로커를 설정 |
    |  |  | registerStompEndpoints |  |
    |  | Controller |  |  |
- 코드
    - WebSocketBrokerConfig.java
        
        ```java
        import org.springframework.context.annotation.Configuration;
        import org.springframework.messaging.simp.config.MessageBrokerRegistry;
        import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
        import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
        import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;
        
        @Configuration
        @EnableWebSocketMessageBroker
        public class WebSocketBrokerConfig implements WebSocketMessageBrokerConfigurer {
        	
        	/*
        	 * STOMP에서 사용하는 메시지브로커를 설정하는 부분
        	 */
        	@Override
        	public void configureMessageBroker(MessageBrokerRegistry registry) {
        		registry.enableSimpleBroker("/queue", "/topic");
        		registry.setApplicationDestinationPrefixes("/app");
        	}
        	
        	@Override
        	public void registerStompEndpoints(StompEndpointRegistry registry) {
        		registry.addEndpoint("/gs-guide-websocket").withSockJS();
        	}
        
        }
        ```
        
        | 구분 | 상세 |
        | --- | --- |
        | enableSimpleBroker | - 스프링의 내장 브로커를 사용 <br> - 송신된 메시지의 prefix가 파라미터 값인 경우에 이를 메시지 브로커가 처리 <br> - 통상적으로 .. <br>    - “/queue” : 메시지 1:1 송신 <br>    - “/topic” : 메시지 1:N으로 여러 명에게 broadcasting |
        | setApplicationDestinationPrefixes | - 메시지의 처리나 가공이 필요한 경우 handler를 사용 <br> - 파라미터 값이 prefix로 들어오면, 이 경로를 처리하는 handler로 전달 |
        | registerStompEndpoints | - WebSocket의 addHandler와 유사 <br> - 파라미터는 처음 웹소켓의 핸드쉐이크 위한 주소 <br> - CORS 설정, SockJS 설정 가능 <br> - WebSocket 방식과 다르게 Handler 따로 설정하지 않고 Controller 방식으로 간편하게 사용할 수 있음 |
    - Controller.java <br> - WebSocketBrokerConfig의 Handler에서 configureMessageBroker의 .setApplicationDestinationPrefixes에서 handler를 구현 <br> - STOMP 사용하면 따로 상속 받지 않고 `@Controller` Annotation으로 사용
        
        ```java
        import org.springframework.messaging.handler.annotation.MessageMapping;
        import org.springframework.messaging.handler.annotation.SendTo;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.util.HtmlUtils;
        
        @Controller
        public class GreetingController {
        	
        	@MessageMapping("/hello")
        	@SendTo("/topic/greeting") // handler에서 처리를 마친 반환값을 topic/greeting이라는 경로로 다시 메시지를 보내겠다. 처리르 마치고 반환된 greeting 객체를 topic/greeting으로 다시 보내는데, 
        	// 앞에 topic이 붙어서 simpleBroker로 돌아오게 될 것
        	public Greeting greeting(HelloMessage message) throws Exception {
        		Thread.sleep(1000);
        		return new Greeting(
        			"Hello, " + HtmlUtils.htmlEscape(message.getName()) + "!"
        		);
        	}
        
        }
        ```
        
        | 구분 | 상세 |
        | --- | --- |
        | @MessageMapping(”/hello”) | - RequestMapping과 비슷한 역할 <br> - STOMP WebSocket 통신 통해 메시지가 들어왔을 때, 메시지의 destination header와 message mapping의 경로가 일치한 핸들러를 찾고 핸들러가 처리 <br> - 여기에서는 configuration에서 설정한 app이라는 prefix와 합쳐져서 app/hello라는 destination 가진 메시지들이 이 핸들러를 거치게 될 것임 |
        | @SendTo("/topic/greeting") | - handler에서 처리를 마친 반환값을 topic/greeting이라는 경로로 다시 보냄 <br> - 처리를 마치고 반환된 greeting 객체를 topic/greeting으로 다시 보내는데, 앞에 topic이 붙어서 simpleBroker로 돌아오게 됨 |

<br>
<br>

## **STOMP의 장점**

<br>

- 하위 프로토콜 혹은 컨벤션을 따로 정의할 필요 없음 (STOMP가 구현해주므로)
- 연결 주소마다 새로 핸들러를 구현하고 설정해줄 필요 없음
    - 핸들러 또한 기존에 사용하던 `@Controller` Annotation 사용하는 등 익숙한 방식으로 사용 가능
- 외부 Messaging Queue를 사용할 수 있음 (RabbitMQ, 카프카..)
    - 이 예시에서는 내부 Messaging Queue 사용 ..!
- Spring Security를 사용할 수 있음

<br>

---

**참고 자료**

<br>

- [[10분 테코톡] ✨ 아론의 웹소켓&스프링](https://www.youtube.com/watch?v=rvss-_t6gzg)
- [WebSocket으로 실시간 채팅 구현하기.(springboot)](https://compogetters.tistory.com/96)
    
<br>
<br>
