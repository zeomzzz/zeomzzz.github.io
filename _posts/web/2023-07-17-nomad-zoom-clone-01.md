---
title:  "[NomadCoders Zoom Clone] 01. Chat with WebSockets"
excerpt: ""

categories:
  - Web
tags:
  - [Web, Lecture, WebSocket]

toc: true
toc_sticky: true
 
date: 2023-07-17
last_modified_at: 2023-07-17
---

<br>

# **01. Chat with WebSockets**

<br>

## **1.0. Introduction**

<br>

## **1.1. HTTP vs WebSockets**

<br>

### **HTTP vs. WebSockets**

<br>

- 공통점 : 프로토콜 (protocol)
- 차이점
    
    
    | HTTP | WebSockets |
    | --- | --- |
    | stateless | 연결이 성립되면 bi-directional한 연결 |
    | 클라이언트는 request만,
    서버는 response만 | 메시지를 보낼 때, request, response가 필요하지 않음 |

<br>

#### **Http**

<br>

- 유저가 request 보내면 서버가 response .. 이걸 반복
    
    ex.
    
    ```jsx
    app.get("/", (req, res) => res.render("home"));
    app.get("/*", (req, res) => res.redirect("/"));
    ```
    
- 특징
    - stateless : real-time X
        - request-response가 끝나면 Backend가 사용자를 기억하지 못함 (사용자와 Back 사이에 연결 X)
        - 서버는 request 받을 때만 response
        - 서버로 메시지를 보내고 싶은데 로그인 되어있으면 사용자 정보를 담은 cookie를 보내면 됨

<br>
<br>

#### **WebSocket**

<br>

- 실시간 기능을 가능하게 함
- http와는 다른 프로토콜
    - WebSocket 지원하는 서버에 WebSocket 사용해서 연결하려면 `wss:// ~` (Secure Web Socket)
- WebSocket과 real-time
    - 연결(connection)이 일어날 때 handshake
    - handshake가 한 번 성립되면, 연결이 성립
    - 연결되었기 때문에 (bi-directional)
        - 서버는 사용자를 기억
        - request를 기다리지 않고도 서버가 유저에게 메세지를 보낼 수 있음 : request, response의 과정이 필요하지 않음
    - backend와 브라우저 뿐만 아니라 두 개의 backend 사이에서도 가능

<br>
<br>

## **1.2. WebSockets in NodeJS**

<br>
<br>