---
title:  "JWT Interceptor"
excerpt: ""

categories:
  - Web
tags:
  - [Auth, JWT, Interceptor]

toc: true
toc_sticky: true
 
date: 2023-11-25
last_modified_at: 2023-11-25
---

<br>
<div class="descipt-font">
JWT 토큰 검증 로직을 검증이 필요한 api 요청 로직에 각각에 넣으면 같은 코드가 여러 번 반복되고 관리가 번거롭다. <br>
Interceptor를 이용해 JWT 토큰 검증을 해보았다.
</div>
<br>

# **1. Interceptor 생성**

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import lombok.RequiredArgsConstructor;
import org.apache.http.HttpHeaders;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;

@Component
@RequiredArgsConstructor
public class JwtInterceptor implements HandlerInterceptor {

    private final JwtUtil jwtUtil;
    private static final Logger logger = LoggerFactory.getLogger(JwtInterceptor.class);

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) 
    throws Exception {
        String uri = request.getRequestURI();
        if (request.getMethod().equals("OPTIONS")) {
            return true;
        }

        String bearerToken = request.getHeader(HttpHeaders.AUTHORIZATION);
        String token = jwtUtil.removeBearer(bearerToken);

        if (token != null) {
            if (jwtUtil.validateToken(token) == true) {
                return true;
            }
        }

        throw new InvalidJwtTokenException("유효하지 않은 토큰입니다.");
    }
}
```

<br>

# **2. Interceptor 등록**

```java
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.*;

@Configuration
@EnableWebMvc
@RequiredArgsConstructor
public class WebMvcConfig implements WebMvcConfigurer {

    private final JwtInterceptor jwtInterceptor;

    public WebMvcConfig(JwtInterceptor jwtInterceptor) {
        this.jwtInterceptor = jwtInterceptor;
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(jwtInterceptor)
                .addPathPatterns("/**");
    }
}
```

<br>

# **3. 제외할 API 등록**

## **1) swagger 제외 - `excludePathPatterns`**

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
		registry.addInterceptor(jwtInterceptor)
				.addPathPatterns("/**")
                .excludePathPatterns("/docs/swagger-ui/**", "/swagger-ui/**", "/docs/swagger-config/**", "/docs/main/**");
}
```

<br>

## **2) 그 외의 인가 작업에서 제외할 API - `@NoAuth`**

### **`@NoAuth` 어노테이션 생성**

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface NoAuth {
}
```

- `@Target(ElementType.METHOD)`
    - 어노테이션이 적용될 수 있는 대상을 지정
    - `ElementType.METHOD` : 메서드에만 적용될 수 있음
    - 이 외의 다른 대상으로는 클래스, 필드 등
- `@Retention(RetentionPolicy.RUNTIME)`
    - 어노테이션이 언제까지 유지될 것인지를 지정
    - `RetentionPolicy.RUNTIME` : 런타임 시에도 이 어노테이션이 유지
    - 이 외에는 `RetentionPolicy.SOURCE`, `RetentionPolicy.CLASS`
        - `SOURCE`는 소스 코드까지만 유지
        - `CLASS`는 컴파일 시까지 유지

<br>

### **JWT Interceptor에 `@NoAuth` Annotation 인가 제외 처리**

```java
if (handler instanceof HandlerMethod) {
		HandlerMethod handlerMethod = (HandlerMethod) handler;
    Method method = handlerMethod.getMethod();

    if (method.isAnnotationPresent(NoAuth.class)) {
		return true;
    }
}
```

- `if (handler instanceof HandlerMethod)`
    - 현재 요청이 실제로 컨트롤러 메소드를 처리하는 `HandlerMethod` 클래스의 인스턴스인지 확인
    - `HandlerMethod`
        - Encapsulates information about a handler method consisting of a method and a bean
        - `@RequestMapping`과 그 하위 어노테이션(`@GetMapping`, `@PostMapping` 등)이 붙은 메소드의 정보를 추상화한 객체
        - Spring MVC에서 요청이 들어오면 `HandlerInterceptor`가 동작
            - `HandlerInterceptor` : 모든 종류의 핸들러에 대해 실행됨
        - 따라서 `HandlerMethod`일 경우에만 특정 작업을 수행하도록 함

<br>

---

**참고 문서**

- [Spring Boot: Interceptor로 토큰 인증 및 사용자 Status 확인](https://sysgongbu.tistory.com/55)
- [스프링부트에서 JWT 적용하기 - 2. 인터셉터(Interceptor)에 적용](https://akku-dev.tistory.com/m/120)
- [JWT을 활용한 로그인/ Interceptor를 활용한 인가 처리 구현](https://mslilsunshine.tistory.com/170)
- [[Spring] 디스패처 서블릿(Dispatcher Servlert)이 HTTP 요청을 처리하는 방법과 핸들러 메소드(HandlerMethod)](https://mangkyu.tistory.com/180)

<br>