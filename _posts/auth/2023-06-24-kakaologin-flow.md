---
title:  "카카오 로그인 흐름 정리"
excerpt: "소셜 로그인 스터디 중 공유한 카카오 로그인 흐름 정리"

categories:
  - Auth
tags:
  - [Auth, KAuth, Social Login]

toc: true
toc_sticky: true
 
date: 2023-06-17
last_modified_at: 2023-07-02
---

<br>

- [관련 Repository](https://github.com/zeomzzz/kauth_practice)

<br>


# **카카오 로그인 흐름**

<br>

![]({{ site.url }}{{ site.baseurl }}/assets/images/login/kakaologin-flow/01.png ){: .align-center}

<br>
<br>

## **Step 1. 인가 코드 받기**

<br>

![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/02.png ){: .align-center}

![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/03.png ){: .align-center}

<br>
1) GET /oath/authorize

- 아래 url로 접속
    
    ```java
    kauth.kakao.com/oauth/authorize?client_id={REST_API_KEY}&redirect_uri={REDIRECT_URI}&response_type=code
    ```
<br> 
2) 카카오계정 로그인 요청 (카카오 인증 서버 -> 사용자)<br>
3) 카카오계정 로그인 (사용자 -> 카카오 인증 서버) <br>
4) 동의 화면 출력 (카카오 인증 서버 -> 사용자) <br>
5) 동의하고 계속하기 (사용자 -> 카카오 인증 서버) <br>
6) 302 Redirect URI로 인가 코드 전달 (카카오 인증 서버 -> 서비스 서버)

- Redirect Url로 리다이렉트
    
    ex. `localhost:8080/oauth/kakao?code=<authentication code>`
    
    - Query String의 code가 Authentication Code
    
    
    >💡 **Authentication Code를 추출해보자**
    >
    >OAuthController.java
    >
    >```java
    >@RestController
    >@AllArgsConstructor
    >@RequestMapping("/oauth")
    >public class OAuthController {
    >
    >    @ResponseBody
    >    @GetMapping("/kakao")
    >    public void kakaoCallback(@RequestParam String code) {
    >        System.out.println(code); // Authentication Code 출력
    >    }
    >}
    >```

<br>
<br>

## **Step 2. 토큰(Access Token) 받기**

<br>

![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/02.png ){: .align-center}

![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/05.png ){: .align-center}
<br>0) Backend 코드 작성

<br>

(1) JSON 파싱 라이브러리 추가

- GSON 또는 Jackson(SpringBoot 자동 제공)
- POST로 요청하고 결과를 JSON으로 받아오기 때문

<br>

(2) Access Token 발급 받는 Service.java

- 구조
    1. connction 생성
        
        ![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/06.png ){: .align-center}
        
    2. POST를 보낼 Body 생성
        
        ![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/07.png ){: .align-center}
        
    3. 받아온 결과 JSON 파싱
        - 성공 시, 응답 코드 200
            
            ex. 
            
            ![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/08.png ){: .align-center}
            
<br>

OAuthService.java (*GSON 이용)

```java
import com.google.gson.JsonParser;
import com.google.gson.JsonElement;
import org.springframework.stereotype.Service;

import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;

@Service
public class OAuthService{

    public String getKakaoAccessToken (String code) {
        String access_Token = "";
        String refresh_Token = "";
        String reqURL = "https://kauth.kakao.com/oauth/token";

        try {

			/* 1. Connection 생성 */
            URL url = new URL(reqURL);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();

            //POST 요청을 위해 setDoOutput을 true (기본값 false)
            conn.setRequestMethod("POST");
            conn.setDoOutput(true);

			/* 2. POST를 보낼 Body 생성 */
            //POST 요청에 필요한 파라미터 스트림을 통해 전송
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(conn.getOutputStream()));
            StringBuilder sb = new StringBuilder();
            sb.append("grant_type=authorization_code");
            sb.append("&client_id=<kakao에서 발급한 REST API KEY>");
            sb.append("&redirect_uri=<kakao에 등록한 redirect URI>");
            sb.append("&code=" + code);
            bw.write(sb.toString());
            bw.flush();

            //결과 코드가 200이라면 성공 -> 200이 아닌 경우 catch로
            int responseCode = conn.getResponseCode();
            // System.out.println("responseCode : " + responseCode);

			/* 3. 받아온 결과 JSON 파싱 */
            //요청을 통해 얻은 JSON타입의 Response 읽어오기
            BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            String line = "";
            String result = "";

            while ((line = br.readLine()) != null) {
                result += line;
            }
            // System.out.println("response body : " + result);

            //Gson JSON파싱 객체 생성
            JsonParser parser = new JsonParser();
            JsonElement element = parser.parse(result);

            access_Token = element.getAsJsonObject().get("access_token").getAsString();
            refresh_Token = element.getAsJsonObject().get("refresh_token").getAsString();

            // System.out.println("access_token : " + access_Token);
            // System.out.println("refresh_token : " + refresh_Token);

            br.close();
            bw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return access_Token;
    }
}
```

<br>

1) POST /oauth/token

- 메서드 : POST
- URL : `https://kauth.kakao.com/oauth/token`

<br>

2) 토큰 발급

<br>
<br>

## **Step 3. 사용자 로그인 처리**

<br>

![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/02.png ){: .align-center}

![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/09.png ){: .align-center}

<br>1) 발급받은 토큰으로 사용자 정보 조회

![]({{ site.url }}{{ site.baseurl }}/assets/images/auth/kakaologin-flow/10.png ){: .align-center}

>🚨 Bearer 뒤에 공백 !!

<br>

- 코드 예시
    - 구조
        1. Connection 생성
        2. 결과 JOSN 파싱

```java
public void createKakaoUser(String token) throws BaseException {

	String reqURL = "https://kapi.kakao.com/v2/user/me";

    //access_token을 이용하여 사용자 정보 조회
    try {

	    /* 1. Connection 생성 */
       URL url = new URL(reqURL);
       HttpURLConnection conn = (HttpURLConnection) url.openConnection();

       conn.setRequestMethod("POST");
       conn.setDoOutput(true);
       conn.setRequestProperty("Authorization", "Bearer " + token); //전송할 header 작성, access_token전송

       //결과 코드가 200이라면 성공 -> 200 아니면 catch로
       int responseCode = conn.getResponseCode();
       System.out.println("responseCode : " + responseCode);

       //요청을 통해 얻은 JSON타입의 Response 메세지 읽어오기
       BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
       String line = "";
       String result = "";

       while ((line = br.readLine()) != null) {
           result += line;
       }
       // System.out.println("response body : " + result);

	    /* 2. 받아온 결과 JSON 파싱 */
       //Gson 라이브러리로 JSON파싱
       JsonParser parser = new JsonParser();
       JsonElement element = parser.parse(result);

       String id = element.getAsJsonObject().get("id").getAsString();
       boolean hasEmail = element.getAsJsonObject().get("kakao_account").getAsJsonObject().get("has_email").getAsBoolean();
       String email = "";
       if(hasEmail){
           email = element.getAsJsonObject().get("kakao_account").getAsJsonObject().get("email").getAsString();
       }

       // System.out.println("id : " + id);
       // System.out.println("email : " + email);

       br.close();

       } catch (IOException e) {
            e.printStackTrace();
       }
 }
```

(2) 응답 받은 정보 이용하여 자체적으로 회원가입 또는 로그인

<br>

---

<br>

**참고자료**

<br>

- [카카오 로그인 공식 문서](https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#req-user-info)
- [[Spring Boot] 카카오 로그인 API 구현 (1) - Access token 발급받기](https://suyeoniii.tistory.com/79)
- [[Spring Boot] 카카오 로그인 API 구현 (2) - Access token으로 사용자 정보 가져오기](https://suyeoniii.tistory.com/81)

<br>
<br>