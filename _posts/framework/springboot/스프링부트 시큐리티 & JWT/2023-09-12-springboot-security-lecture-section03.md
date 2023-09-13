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

# **19강. JWT를 위한 yml 파일 세팅**

<br>

## **yml 예시**


```yaml
server:
  port: 8080
  servlet:
    context-path: /
    encoding:
      charset: UTF-8
      enabled: true
      force: true
      
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/security?serverTimezone=Asia/Seoul
    username: // username
    password: // password

  jpa:
    hibernate:
      ddl-auto: create #create update none
      naming:
        physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
    show-sql: true
```

<br>

## **Controller**

```java
package com.cos.jwt.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class RestApiController {
	
	@GetMapping("home")
	public String home() {
		return "<h1>home</h1>";
	}
	
}
```

- localhost 8080 으로 접속하면 로그인 화면이 뜸
    
    ![]({{ site.url }}{{ site.baseurl }}/assets/images/springboot/security-lecture-section03/01.png ){: .align-center}
    
    - Username : user
    - Password : SpringBoot 실행 시 생성된 security password

<br>

# **20강. jwt를 위한 security 설정**

<br>

## **User Model 생성**

```java
package com.cos.jwt.model;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import jakarta.persistence.Id;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import lombok.Data;

@Data
@Entity
public class User {
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;
	private String username;
	private String password;
	private String roles; // USER, ADMIN
	
	public List<String> getRoleList(){
		if(this.roles.length() > 0) {
			return Arrays.asList(this.roles.split("."));
		}
		return new ArrayList<>();
	}

}
```

>🚨 Entity '__' has no identifier (every '@Entity' class must declare or inherit at least one '@Id' or '@EmbeddedId' property) 에러
>
>- `@Id` Annotation을 `import jakarta.persistence.Id;` 가 아닌 `import org.springframework.data.annotation.Id` 로 import 하지 않았는지 확인!

<br>

## **Config**

### **Security Config**

```java
package com.cos.jwt.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.web.filter.CorsFilter;

import lombok.RequiredArgsConstructor;

@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig extends WebSecurityConfigurerAdapter {
	
	@Autowired
	private final CorsFilter corsFilter;

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http
				.csrf().disable()
				.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS) // 세션을 사용하지 않고 stateless 서버로 만들겠다
			.and()
				.addFilter(corsFilter) // 이 필터를 타야 서버가 cors 정책에서 벗어날 수 있음
				.formLogin().disable() // formLogin 안 쓸거니까 disable (JWT 로그인할 거니까)
				
				.httpBasic().disable()
				.authorizeRequests()
				.antMatchers("/api/v1/user/**")
					.access("hasRole('ROLE_USER') or hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')")
				.antMatchers("/api/v1/manager/**")
					.access("hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')")
				.antMatchers("/api/v1/admin/**")
					.access("hasRole('ROLE_ADMIN')")
				.anyRequest().permitAll(); // 다른 요청은 모두 허용
		
	}
	
}
```

- `@CrossOrigin` vs. `CorsFilter`
    - `@CrossOrigin` : 인증이 없는 경우
    - `CorsFilter` : 인증이 있는 경우. Security Filter에 등록 해주어야 함
- 이제 실행하면 로그인 하지 않고도 home에 접속 가능 : 세션을 사용하지 않아서 모든 페이지로 접근이 가능해짐

<br>

### **CorsConfig**

```java
package com.cos.jwt.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;

@Configuration
public class CorsConfig {

	@Bean
	public CorsFilter corsFilter() {
		UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
		CorsConfiguration config = new CorsConfiguration();
		config.setAllowCredentials(true); // 내 서버가 응답할 때 json을 js에서 처리할 수 있게 할 지를 설정 (ajax, axios 요청 등을 js에서 받을 수 있도록)
		config.addAllowedOrigin("*"); // origin을 어디에서든 허용
		config.addAllowedHeader("*"); // 모든 header를 허용
		config.addAllowedMethod("*"); // 모든 요청(post, get, put, delete, patch)을 허용
		source.registerCorsConfiguration("/api/**", config); // /api/**로 들어오는 모든 주소를 이 config로 설정한다고 source에 등록  
		return new CorsFilter(source);
	}
}
```

<br>