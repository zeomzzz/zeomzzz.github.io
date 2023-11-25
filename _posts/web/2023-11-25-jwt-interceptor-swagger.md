---
title:  "JWT Interceptor에서 Swagger 제외하기"
excerpt: ""

categories:
  - Web
tags:
  - [Auth, JWT, Interceptor, Swagger, TroubleShooting]

toc: true
toc_sticky: true
 
date: 2023-11-25
last_modified_at: 2023-11-25
---

<br>

# **1. 문제 상황**

- JWT Interceptor 적용 후 swagger에 접속하면 오류 메시지가 뜨고 접속할 수 없음
    
    ![]({{ site.url }}{{ site.baseurl }}/assets/images/web/jwt-interceptor-swagger/01.png ){: .align-center}
    
<br>

# **2. 원인 파악**

- swagger를 포함한 모든 경로에서 JWT Interceptor가 동작하고 있었다
    - swagger 접속시에 JWT Interceptor에 작성해둔 로그가 찍힘
        
        ![]({{ site.url }}{{ site.baseurl }}/assets/images/web/jwt-interceptor-swagger/02.png ){: .align-center}
        
<br>

# **3. 해결 방법**

- WebConfig에 Interceptor를 등록할 때 swagger 경로를 제외한다
- 로그를 출력해서 요청이 가고 있는 경로를 확인 한 후 관련된 경로를 제외하였다

<br>

## **1) 요청 경로 확인 방법**

```java
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse reponse, Object handler) 
throws Exception {
    String uri = request.getRequestURI();
    logger.debug("uri : " + uri.toString());
// ...
    throw new InvalidJwtTokenException("유효하지 않은 토큰입니다.");
}
```

- 확인한 경로들 : `"/docs/swagger-ui/**", "/swagger-ui/**", "/docs/swagger-config/**", "/docs/main/**"`
    
    ![]({{ site.url }}{{ site.baseurl }}/assets/images/web/jwt-interceptor-swagger/03.png ){: .align-center}
    
<br>

## **2) Interceptor 등록할 때 swagger 경로 제외**

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
		registry.addInterceptor(jwtInterceptor)
				.addPathPatterns("/**")
                .excludePathPatterns("/docs/swagger-ui/**", "/swagger-ui/**", "/docs/swagger-config/**", "/docs/main/**");
}
```

<br>

## **3) swagger 접속하여 확인**

![]({{ site.url }}{{ site.baseurl }}/assets/images/web/jwt-interceptor-swagger/04.png ){: .align-center}

<br>