---
title:  "RESTful API란?"
excerpt: ""

categories:
  - Computer Science
tags:
  - [Network, REST]

toc: true
toc_sticky: true
 
date: 2023-12-29
last_modified_at: 2023-12-29
---

<br>


>💡 **요약**
>
>- REST란?
>    - HTTP URI를 통해 제어할 자원(Resource)를 명시하고, HTTP Method(GET, POST, PUT, DELETE)을 통해 해당 자원(Resource)를 제어하는 명령을 내리는 방식의 아키텍처
>    - 구성요소
>        - 자원 (Resource) : URI
>        - 행위 (Verb) : HTTP Method
>        - 표현 (Representations) : JSON, XML, TEXT, RSS 등의 메시지 포맷
>- RESTful API란?
>    - REST 기반으로 서비스 API를 구현한 것
>- 장점
>    - HTTP 표준을 기반으로 구현하여, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있음
>    - 클라이언트와 서버 역할을 명확하게 분리
>- 단점
>    - 표준이 존재하지 않음
>    - HTTP Method가 제한적

<br>
<br>

# **1. REST란?**

## **정의**

<br>

- REST(REpresentational State Transfer, 대표적인 상태 전달)
- 월드 와이드 웹(www)과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 개발 아키텍처의 한 형식이며, 네트워크 상에서 Client와 Server 사이의 통신 방식 중 하나
    
  
    >💡 **REST 외의 통신 방식**
    >
    >- SOAP (Simple Object Access Protocol) : XML 기반의 메시지 교환 프로토콜
    >- gRPC (gRPC Remote Procedure Calls) : 구글에서 개발한 오픈 소스 RPC(Remote Procedure Call) 프레임워크
    >- GraphQL (Graph Query Language) : 페이스북에서 개발한 쿼리 언어
    
- 자원 기반의 구조(ROA, Resource Oriented Architecture) 설계의 중심에 Resource가 있으며, HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐
    - 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여
- REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일
    - HTTP의 다양한 메서드(GET, POST, PUT, DELETE 등)를 활용하여 자원에 대한 행위를 정의하고, HTTP 상태 코드를 이용하여 요청의 성공 또는 실패를 표현

<br>
<br>

## **장단점**

<br>

- 장점
    - 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화
    - Hypermedia API의 기본을 충실히 지키면서 범용성을 보장
    - HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해줌
    - 서버와 클라이언트의 역할을 명확하게 분리
- 단점
    - 표준이 존재하지 않음
    - HTTP Method 형태가 제한적
    - 구형 브라우저가 아직 제대로 지원해주지 못하는 부분 존재
        - PUT, DELETE를 사용하지 못함
        - pushState를 지원하지 않음

<br>
<br>

## **구성 요소**

<br>

| 구성요소 | 설명 |
| --- | --- |
| 자원(Resource): URI | - 모든 것을 Resource(명사)로 표현하고, 세부 Resource에는 id를 붙임<br>- 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재<br>- 자원을 구별하는 ID는 '/groups/:group_id'와 같은 HTTP URI<br>- Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청 |
| 행위(Verb): HTTP Method | - HTTP 프로토콜의 Method를 사용 |
| 표현(Representation of Resource) / Message | - JSON, XML, TEXT, RSS 등의 메시지 포맷이 존재
    - JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적임<br>- Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보냄 |

<br>
<br>

## **특징**

<br>

- Server-Client(서버-클라이언트 구조)
    - REST 서버는 API를 제공하고 클라이언트는 사용자 정보를 관리하는 구조로 분리하여 설계
    - 클라이언트와 서버 각 서로에 대한 의존성을 낮추는 기능을 함
- Stateless(무상태)
    - HTTP Session과 같은 컨텍스트 저장소에 상태 정보를 저장하지 않음
        - Server는 Client가 보낸 요청에 대해 Session이나 Cookie 정보를 별도 보관 X
    - Request만 Message로 처리하면 되고, 컨텍스트 정보를 신경쓰지 않아도 되므로, 로직의 자유도가 높고 구현이 단순
    - REST API 실행 중 실패가 발생한 경우, Transaction 복구를 위해 기존의 상태를 저장할 필요가 있음 (POST Method 제외)
- Cacheable(캐시 처리 가능)
    - HTTP 표준을 그대로 사용하므로 HTTP의 캐싱 기능을 적용할 수 있음
    - 캐싱 기능을 사용하기 위해서는 응답과 요청이 모두 캐싱 가능한지(Cacheable) 명시가 필요함
- Layered System(계층화)
    - REST 서버는 네트워크 상의 여러 계층으로 구성될 수 있음
    - 서버의 복잡도와 관계없이 클라이언트는 서버와 연결되는 포인트만 알면 됨
- Code-On-Demand(optional)
- Uniform Interface(인터페이스 일관성)
    - Rest 서버는 H 표준 전송 규약을 따르므로 HTTP 표준만 맞는다면 어떤 프로그래밍 언어로 만들어 졌느냐와 상관없이 플랫폼 및 기술에 종속되지 않고 타 언어, 플랫폼, 기술 등과 호환해서 사용할 수 있음
    - 포함
        - Self-Descriptive Messages
            - API 메시지만 보고, API를 이해할 수 있는 구조
                - Resource, Method를 이용해 무슨 행위를 하는지 직관적으로 알 수 있음
        - HATEOAS(Hypermedia As The Engine Of Application State)
            - Application의 상태(State)는 Hyperlink를 통해 전이되어야 함
            - 서버는 현재 이용 가능한 다른 작업에 대한 하이퍼링크를 포함하여 응답해야 함
        - Resource Identification In Requests
        - Resource Manipulation Through Representations

<br>
<br>

# **2. REST API란?**

## **정의**

<br>

- REST 기반으로 서비스 API를 구현한 것
- 최근 OpenAPI, 마이크로 서비스 등을 제공하는 업체 대부분은 REST API를 제공

<br>
<br>

## **특징**

<br>

- REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운용이 편리
- REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있음

<br>
<br>

## **REST API 설계 기본 규칙**

<br>

1. URI는 정보의 자원을 표현
    - resource는 동사보다는 명사로 영어 소문자 복수형을 사용하여 표현
2. 자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현
    - URI에 HTTP Method나 행위에 대한 동사 표현이 들어가면 안됨
- 그 외의 규칙들
    1. 슬래시 구분자(`/`)는 계층 관계를 나타내는데 사용하며, URI 마지막 문자로는 사용하지 않음
    - URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 함
        - URI가 다름 == 리소스가 다름 -> 리소스가 다르면 URI도 달라져야 함
    - REST API는 분명한 URI를 만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 함
    1. 불가피하게 긴 URI경로를 사용하게 된다면 하이픈(`-`)을 사용해 가독성을 높임
    2. 언더바(`_`)는 URI에 사용하지 않음
        - 보기 어렵거나 문자를 가리기도하므로 가독성을 위해 사용하지 않음
    3. URI 경로에는 소문자가 적합하며, 대문자 사용은 피함
        - RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정
    4. 파일확장자는 URI에 포함하지 않음
        - Accept header를 사용
            
            ```
            http://restapi.example.com/members/soccer/345/photo.jpg (X)
            GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg (O)
            ```
            
    5. 리소스 간에 연관 관계가 있는 경우 : `/리소스명/리소스 ID/관계가 있는 다른 리소스명`
    6. `:id`는 하나의 특정 resource를 나타내는 고유값

<br>
<br>

---

**참고 자료**

<br>

- [Interview_Question_for_Beginner RESTful API](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/main/Development_common_sense#restful-api)
- [WeareSoft/tech-interview REST와 RESTful의 개념](https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md#rest%EC%99%80-restful%EC%9D%98-%EA%B0%9C%EB%85%90)

<br>
<br>