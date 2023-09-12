---
title:  "[스프링부트 시큐리티 & JWT] 섹션 3. 스프링 시큐리티 JWT 서버 구축"
excerpt: ""

categories:
  - SpringBoot
tags:
  - [Framework, Lecture, SpringSecurity, JWT]

toc: true
toc_sticky: true
 
date: 2023-09-12
last_modified_at: 2023-09-12
---

<br>

# **17강. JWT 구조의 이해**

<br>

## **JWT 토큰이란?**

- JSON Web Token


> JWT(JSON 웹 토큰)는 당사자 간에 정보를 JSON 개체로 안전하게 전송하기 위한 간결하고 독립적인 방법을 정의하는 개방형 표준(RFC 7519)입니다. 이 정보는 디지털 서명이 되어 있으므로 확인하고 신뢰할 수 있습니다. JWT는 비밀(HMAC 알고리즘 사용) 또는 RSA 또는 ECDSA를 사용하는 공개/개인 키 쌍을 사용하여 서명할 수 있습니다.
>
>JWT를 암호화하여 당사자 간 보안을 제공할 수도 있지만 우리는 **서명된 토큰에 중점**을 둘 것입니다. 서명된 토큰은 그 안에 포함된 청구(claim)의 무결성을 확인할 수 있는 반면, 암호화된 토큰은 이러한 청구를 다른 당사자로부터 숨깁니다. 공개/개인 키 쌍을 사용하여 토큰에 서명하는 경우 서명은 개인 키를 보유하고 있는 당사자만이 서명한 사람임을 인증합니다.

<br>

## **JWT 토큰의 구조**

```
xxxxx.yyyyy.zzzzz
```

- xxxxx : Header
- yyyyy : Payload ( 어떤 정보)
- zzzzz : Signature

<br>

### **Header**

```
{
  "alg": "HS256", // 사용한 알고리즘
  "typ": "JWT"
}
```

- Base64로 인코딩 됨
    - Base64 : 디코딩 가능 → 암호화에 목적이 있는게 아니라 서명에 목적. 데이터가 유효한지 무결성을 보장하기 위한 목적

<br>

### **Payload**

```
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

- claim을 가지고 있음
    
    
    | 클레임 | 설명 |
    | --- | --- |
    | 등록된 클레임 | - 필수는 아니지만 권장되는 클레임의 집합 <br> - token 발행자, 만료시간, 주제 등 |
    | 개인 클레임 | - user id 등을 넣음 <br> - 공개되어도 되지만 그 유저를 특정할 수 있는 키를 넣음 |

<br>

### **서명**

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

- header, payload, 개인 key의 3개의 key를 HS256(Header에 명시)로 암호화
    - HS256 == HMACSHA256
- secret : 서버만 알고 있는 key

<br>

## **Flow 예시**

1. 클라에서 id, pw로 서버에 로그인 시도
2. 서버가 header, payload, signature를 만듦
    - header : signature에 HS256으로 서명함
    - payload : user name
    - signature : header + payload + secret(서버만 앎)을 HS256으로 암호화
        - HS256 : HMAC이라는 방식으로 SHA256 암호화 (해시 암호화 : 복호화 불가능)
            - HMAC : 어떤 secret key를 포함하는 암호화 방식
3. header, payload, signature를 각각 Base64로 암호화해서 클라에 돌려줌
4. 클라이언트는 이 정보를 로컬스토리지와 같은 웹 브라우저 저장소에 저장
5. 클라가 서버에 개인 정보를 요청. 이 때, 로컬 스토리지에 저장한 JWT를 함께 보냄
6. 서버에서 이 JWT를 검증. Signature에 HS256으로 암호화된 정보가 서버에서 알고 있는 header, payload, secret을 HS256으로 암호화해서 동일한지 확인. 이게 같으면 인증
    - 개인정보는 payload에 user name을 이용해서 찾음

- 만일 HS256이 아닌 RSA를 사용한다면?
    - header에 RSA를 사용하였음을 명시
    - payload에 username을 입력
    - signature : header, payload만 이용. secret을 사용하지 않음
        - RSA는 이걸 개인키로 잠궈버리고 토큰을 돌려줌
            
            → 클라가 서버로 요처했을 때 서버는 공개키로 Signature를 검증
            
        - 토큰을 클라가 받아서 서버에 요청 보낼 때

<br>

# **18강. JWT 프로젝트 세팅**

<br>

## **프로젝트 세팅**

>💡 프로젝트 환경
>
>- SpringBoot
>- Java 8
>- Build : maven
>- [Java JWT 3.10.2](https://mvnrepository.com/artifact/com.auth0/java-jwt/3.10.2)

<br>

- Dependency (pom.xml)
    - SpringBootStarter에서 설정 : Lombok, Spring Boot DevTools, Spring Web, Spring Data JPA, MySQL Driver, Spring Security
        - OAuth 2.0을 같이 사용하면 설정을 다르게 해주어야 함 !!
    - Java JWT : JWT token을 만들어줌
        
        ```
        <!-- https://mvnrepository.com/artifact/com.auth0/java-jwt -->
        <dependency>
            <groupId>com.auth0</groupId>
            <artifactId>java-jwt</artifactId>
            <version>3.10.2</version>
        </dependency>
        ```

<br>