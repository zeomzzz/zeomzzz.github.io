---
title:  "JWT (Json Web Token)"
excerpt: ""

categories:
  - Web
tags:
  - [Auth, JWT]

toc: true
toc_sticky: true
 
date: 2023-11-12
last_modified_at: 2023-11-12
---

<br>

# **1. JWT란?**

- JWT : JSON Web Token
    - JSON : Key, Value가 한 쌍을 이루는 객체
    - [공식 사이트](https://jwt.io/)
- 사용하는 이유 Cookie와 Session 기반 인증의 단점을 보완
    - Cookie
        - 단점
            - 브라우저가 종료되어도 만료 시점이 지나면 자동삭제 되지 않음
    - Session
        - 인증정보는 서버에 저장하고, 클라이언트 쿠키에 JSESSIONID를 저장. 요청을 보낼 때 JSESSIONID가 포함된 쿠키를 함께 보냄
        - 단점
            - 유저의 인증 정보를 서버에 저장하여 서버가 Stateful 해야 함
            - 유저의 수가 증가하면 서버의 부하가 심해짐
- JWT의 장/단점
    - 장점
        - Stateless 하여 서버 확장성(Scalability)가 있음
        - 보안성 : 쿠키 탈취 및 위조로 인한 취약점 사라짐
        - 확장성 : 로그인 정보가 사용되는 분야가 확장됨 ex. OAuth
    - 단점
        - 토큰의 길이가 길어서 인증 요청이 많아지면 네트워크 부하가 증가
        - Payload는 암호화되지 않아 유저의 민감한 정보는 담지 못함
        - 토큰 탈취시 대처가 어려움
            - 한 번 발급된 토큰은 유효기간이 만료될 때까지 계속 사용 가능
            - 서버에서 삭제할 수 있는 쿠키/세션과 달리 특정 사용자의 접속을 강제로 만료하기 어려움
    - 단점 극복 방안
        - Refresh Token 활용
            - 클라이언트가 요청시 사용하는 Access Token의 만료 기간을 짧게 설정하고, Refresh Token을 이용해 Access Token 재발급


<br>

# 2. **JWT의 구조**

```xml
HEADER.PAYLOAD.SIGNATURE
```

- Header(헤더), Payload(내용), Signature(서명)이 .(dot)을 구분자로 하여 토큰을 이룸

<br>

## **1) Header : Algorithm, Token Type**

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

- 해시 알고리즘
- 토큰의 타입 정의
- 이 정보를 Base64로 인코딩

<br>

## **2) Payload : Data**

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
```

- 민감한 정보는 Payload에 담으면 안됨
    - Base64로 인코딩되어 누구나 디코딩할 수 있음
- Claim
    - Payload의 Key, Value로 된 각각의 쌍
    - 종류
        - Registered Claim (등록된 클레임)
            - 3글자로 정의
            - 필수는 아니지만 사용이 권장됨
            - [종류](https://datatracker.ietf.org/doc/html/rfc7519#section-4.1) : iss(issuer), exp(expiration time), sub(subject), aud(audience) 등
        - Public Claim (공개 클레임)
            - 사용자가 자유롭게 정의할 수 있음
            - 다만, 충돌을 방지하기 위해 [IANA JSON Web Token Registries에 정의](https://www.iana.org/assignments/jwt/jwt.xhtml)되어 있거나 충돌이 방지된 네임스페이스를 포함하는 URI로 정의
                - URI로 정리하는 예시
                    
                    ```json
                    {"http(s)://specific.uri/@unique_recommended_namespace/additional_path": true}
                    ```
                    
        - Private Claim (비공개 클레임)
            - Registered Claim, Public Claim이 아닌 클레임
            - 정보를 공유하기 위해 만들어진 클레임
            - 이름 중복으로 인한 충돌에 유의해서 사용

<br>

## **3) Signature**

```json
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
)
```

- Header(Base64로 인코딩) + “.” + Payload(Base64로 인코딩)
- Secret
- 이 정보를 Base64로 인코딩
- HMAC : Secret Key를 포함하는 암호화 방식

<br>

# **3. JWT 사용하기**

- 요청 Header의 Authorization에 담으며, Type을 같이 적어줌
- Type은 일반적으로 Bearer을 사용
- 클라이언트의 JWT 저장 장소 : LocalStorage

<br>

---

**참고 자료**

<br>

- [JWT란 무엇인가? 그리고 어떻게 사용하는가? (1) - 개념](https://velog.io/@vamos_eon/JWT%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80-1)

<br>