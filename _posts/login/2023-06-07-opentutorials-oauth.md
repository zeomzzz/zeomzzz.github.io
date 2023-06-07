---
title:  "OAuth 2.0 생활코딩"
excerpt: "생활코딩 OAuth 2.0 필기"

categories:
  - Login
tags:
  - [Login, OAuth, Lecture]

toc: true
toc_sticky: true
 
date: 2023-06-07
last_modified_at: 2023-06-07
---

<br>

# **생활코딩 OAuth 2.0**

<br>

## **1강. 수업 소개**



### **왜 oAuth를 사용할까?**

<br>

- oAuth 이용하면 안전하게 우리가 만든 서비스와 다른 서비스가 상호작용할 수 있음
- 이전 : 아이디/비번을 나의 서비스에서 가지고 있었어야 함
- oAuth : 이제는 유저의 요청에 의해 그들의 서비스가 아이디 비밀번호 대신 access token이라는 일종의 비밀번호를 발급

<br>
<br>


### **oAuth의 장점**

<br>

1. 다른 서비스의 아이디/비밀번호를 가지고 있지 않아도 됨
2. 그들의 서비스가 갖고 있는 모든 기능이 아닌 나의 서비스가 필수적으로 필요한 비밀번호를 부분적으로 제공할 수 있음

<br>
<br>

### **oAuth 사용 기본 원리**

<br>

- oAuth를 통해 access Token을 획득 → 그 access Token을 통해 다른 서비스에 접근 → 데이터 CRUD 할 수 있게 된다.
- 이에 따라 회원의 아이디/비밀번호 없이도 회원을 식별할 수 있음
    - 이를 federated identity라고 하는데, 이것의 가장 기반에 있는 기술이 oAuth

<br>
<br>

## **2강. 역할**

<br>

### **oAuth에 등장하는 3자**

<br>

| 명칭 | 역할 |
| --- | --- |
| Resource Server | 우리가 제어하고자 하는 자원을 가지고 있는 서버 (Their) |
| Resource Owner | 자원의 소유자 (User) |
| Client | Resource Server에 접속해서 정보를 가지고 가는 Client (우리의 서비스) |

```
💡 Authorization Server?

- 인증과 관련된 처리를 전담하는 서버
- 공식 문서는 Resource Server와 구분하여 사용하지만, 본 강의에서는 구분하지 않음
```

<br>
<br>


## **3강. 등록**



### **oAuth 등록 절차 1. 등록**

<br>

- 등록 : Client가 Resource Server를 이용하기 위해 Resource Server의 승인을 사전에 받는 것
- 서비스 마다 등록하는 방법이 상이함. 그렇지만 공통점 세 가지
    
    : Client ID, Client Secret, Authorized Redirect URIs (명칭이 다를 수 있음)
    
    | 요소 | 설명 |
    | --- | --- |
    | Client ID | 우리가 만들고 있는 애플리케이션을 식별하는 식별자. id (노출되어도 됨) |
    | Client Secret | 그것에 대한 비밀번호 (노출되면 안됨) |
    | Authorized redirect URIs | 리소스 서버가 권한을 부여하는 과정에서 authorize code를 전달해줄건데, 이 주소로 전달해달라고 알려주는 것
    따라서 다른 곳에서 요청하면 무시함 |

<br>
<br>

## **4강. Resource Owner의 승인**



### **Resource Owner의 승인을 받는 절차**

<br>

- 등록 후 : Resource Sever, Client 모두 Client Id, Client Secret, Redirect URI 정보를 가지고 있음
    - Redirect URI는 Client가 미리 구현해둔 상태

ex. Resource Server의 기능 A, B, C, D 중 Client는 B, C의 기능만 필요

→ 모든 기능이 아닌 최소한의 기능(B, C)에 대해서만 인증을 받는 것이 좋음

1. Resource Owner가 Client에 접속하는데, Resource Server를 사용해야하는 상황이 발생 ex. Facebook에 글 작성, Google Calendar에 날짜 작성 등
2. Resource Owner의 동의가 필요하다는 화면이 표시됨 ex. Google 로그인 또는 인증이 필요하다는 메시지
    
    → 이때, 버튼을 클릭하면 사용자가 동의한 것
    
    이 버튼은 Client Id, Scope(사용하고자 하는 기능의 범위), Redirect URL이 들어있음
    
    ```java
    https://resource.server/
          ?client_id=1
          &scope=B,C
          &redirect_uri=https://client/callback
    ```
    
    ex. inflearn Google 로그인의 url을 decode
    
    ```java
    https://accounts.google.com/o/oauth2/v2/auth/oauthchooseaccount
    ?access_type=offline&prompt=consent
    &scope=https://www.googleapis.com/auth/userinfo.email https://www.googleapis.com/auth/userinfo.profile&response_type=code
    &client_id=887875630717-ror9t8ig4obhvokdij07eoochpqbu5kf.apps.googleusercontent.com
    &redirect_uri=https://www.inflearn.com/auth/google&state={"prev_url":"https://www.inflearn.com/user/signin?referUrl=https%3A%2F%2Fwww.inflearn.com%2F"}&service=lso&o2v=2&flowName=GeneralOAuthFlow
    ```
    
3. Resource Owner가 링크로 접속하면, Resource Server를 로그인이 되어있는지 보고, 로그인 되어있지 않으면 로그인 창을 보여줌
4. 로그인하면 Resource Server는 다음을 확인
    - 주소의 Client Id와 같은 Client Id가 있는지
    - Redirect URL이 같은지
    
    → 다르면 작업 종료, 같으면 다음 단계로
    
5. 확인이 되면, Resource Owner에게 Scope에 해당하는 권한을 Client에게 부여할 것인지 확인하는 메시지를 전송
6. Resource Owner가 허용 버튼을 클릭하면 Resource Server에 유저가 허용에 동의했다는 정보가 전달됨
    
    ex. 
    
    ```
    Client id : 1
    Client Secret : 2
    redirect URL : https://client/callback
    **user id : 1
    scope : b, c**
    ```
    

<br>
<br>

## **5강. Resource Server의 승인**

<br>

- 지금까지 한 것 : Resource Owner의 승인을 얻기
- 이제해야 하는 것 : Resource Server의 승인을 얻기

<br>
<br>

### **Authorization Code를 이용한 Resource Server의 승인**

<br>

- Authorization Code : Resource Server가 발급해주는 임시 비밀번호

1. Resource Server가 Authorization Code를 발급하여 Resource Owner에게 전송(헤더)
    
    ex. 
    
    ```
    Location: https://client/callback?code=3
    ```
    
    - Resource Server가 Resource Owner의 웹브라우저가 Location 뒤의 주소로 이동하도록 명령
    - `code=3` : Authorization Code
    
    → 이 헤더 값에 의해 Resource Owner가 인식하지 못한 채 이 주소로 이동 & Client는 Authorization Code를 갖게 됨
    
2. Client가 authorization code와 client secret이라는 두 가지 비밀 정보가 결합된 아래 주소를 이용하여 Resource Server에 직접 접속 (Resource Owner 통하지 않고)
    
    ```
    https://resource.server/token?
    grant_type=authorization_code&
    code=3&
    redirect_uri=https://client/callback&
    client_id=1&
    client_secret=2
    ```
    
3. Resource Server는 Authorization Code를 이용하여 이에 해당 하는 정보와 주소의 정보가 모두 동일한지 확인
4. 정보가 모두 일치하면 다음 단계인 Access Token 발급

<br>

```
💡 3자간의 인증 방법 Authorization Code 포함 4가지 있음 !!

```

<br>
<br>

## **6강. 액세스 토큰 발급**

<br>

- 이전 : Resource Server가 Client를 승인
- 이제 할 것 : Access Token 발급 (OAuth의 목적 !!)

<br>
<br>

### **Access Token 발급**

<br>

1. 인증 이후, Resource Server와 Client는 Authorization Code 값을 지움 (그래야 다시 인증 X)
2. Resource Server가 Access Token을 발급하고 이를 Client에 응답 (ex. `AccessToken=4`)
3. Client는 AccessToken을 내부적으로 저장함 ex. DB, 파일 등

<br>
<br>

### **Access Token의 역할**

<br>

- Resource Server는 Access Token에 해당하는 요청이 왔을 때에만 Resource Owner에 대한 scope의 기능을 Client에게 허용함

<br>

## **7강. API 호출**

<br>
<br>

- 지금까지 : Access Token 발급
- 이제 : Access Token 이용해서 Resource Server를 Handling

<br>
<br>

### **Resource Server의 조작 : API**

<br>

- API(Application Programming Interface) : Resource Server를 호출, 조작하기 위한 장치
- 방법 2가지
    1. Access Token을 query parameter로 보냄
    2. Bearer를 Header로 보냄(권장)
        - curl를 이용 (curl : 해당 웹페이지의 정보를 받을 수 있는 프로그램)
        - `-H` 옵션을 이용하여 헤더에 넣음

<br>
<br>

## **8강. Refresh Token**


### **Refresh Token**

<br>

- Access Token에는 수명이 있음. 수명이 다하면 더 이상 Api에 접속해도 Api가 정보를 주지 않음
- 이때, Refresh Token을 이용하여 Access Token을 발급 받을 수 있음
- [Access Token 재발급 절차](https://www.rfc-editor.org/rfc/rfc6749#section-1.5)
    
    ```
      +--------+                                           +---------------+
      |        |--(A)------- Authorization Grant --------->|               |
      |        |                                           |               |
      |        |<-(B)----------- Access Token -------------|               |
      |        |               & Refresh Token             |               |
      |        |                                           |               |
      |        |                            +----------+   |               |
      |        |--(C)---- Access Token ---->|          |   |               |
      |        |                            |          |   |               |
      |        |<-(D)- Protected Resource --| Resource |   | Authorization |
      | Client |                            |  Server  |   |     Server    |
      |        |--(E)---- Access Token ---->|          |   |               |
      |        |                            |          |   |               |
      |        |<-(F)- Invalid Token Error -|          |   |               |
      |        |                            +----------+   |               |
      |        |                                           |               |
      |        |--(G)----------- Refresh Token ----------->|               |
      |        |                                           |               |
      |        |<-(H)----------- Access Token -------------|               |
      +--------+           & Optional Refresh Token        +---------------+
    
                   Figure 2: Refreshing an Expired Access Token
    
       The flow illustrated in Figure 2 includes the following steps:
    
       (A)  The client requests an access token by authenticating with the
            authorization server and presenting an authorization grant.
    
       (B)  The authorization server authenticates the client and validates
            the authorization grant, and if valid, issues an access token
            and a refresh token.
    
       (C)  The client makes a protected resource request to the resource
            server by presenting the access token.
    
       (D)  The resource server validates the access token, and if valid,
            serves the request.
    
       (E)  Steps (C) and (D) repeat until the access token expires.  If the
            client knows the access token expired, it skips to step (G);
            otherwise, it makes another protected resource request.
    
       (F)  Since the access token is invalid, the resource server returns
            an invalid token error.
    ```