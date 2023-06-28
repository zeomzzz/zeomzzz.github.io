---
title:  "Postman을 이용한 API 테스트"
excerpt: ""

categories:
  - Tools
tags:
  - [Tools, Postman]

toc: true
toc_sticky: true
 
date: 2023-06-28
last_modified_at: 2023-06-28
---

<br>


# **1. Postman이란?**

<br>

- [https://www.postman.com/](https://www.postman.com/)
- API를 테스트하여 문서화 및 공유할 수 있는 플랫폼

<br>
<br>

# **2. Postman 테스트 예시**

<br>

## **1) [네이버 지역 검색 API](https://developers.naver.com/docs/serviceapi/search/local/local.md#%EC%A7%80%EC%97%AD-%EA%B2%80%EC%83%89-%EA%B2%B0%EA%B3%BC-%EC%A1%B0%ED%9A%8C)**

<br>

(1) [NAVER Developers](https://developers.naver.com/main/) > Application에 애플리케이션을 등록하고 Client ID, Client Secret 확인

(2) query=검색어, Header에 Client ID, Client Secret 입력 후 Send

![]({{ site.url }}{{ site.baseurl }}/assets/images/tools/postman/naver.png ){: .align-center}

- Naver Developers 요청 예
    
    ```powershell
    curl "https://openapi.naver.com/v1/search/local.xml?query=%EC%A3%BC%EC%8B%9D&display=10&start=1&sort=random" \
        -H "X-Naver-Client-Id: {애플리케이션 등록 시 발급받은 클라이언트 아이디 값}" \
        -H "X-Naver-Client-Secret: {애플리케이션 등록 시 발급받은 클라이언트 시크릿 값}" -v
    ```

<br> 

## **2) [카카오 키워드로 장소 검색하기 API](https://developers.kakao.com/tool/rest-api/open/get/v2-local-search-keyword.%7Bformat%7D)**

<br>

(1) [kakao developers](https://developers.kakao.com/) > 내 애플리케이션에서 애플리케이션 등록하고 REST API 키 확인

(2) query에 검색어, REST API 입력 후 Send

![]({{ site.url }}{{ site.baseurl }}/assets/images/tools/postman/kakao.png ){: .align-center}

- kakao developers 요청 코드 예시
    
    ```powershell
    curl -X GET "https://dapi.kakao.com/v2/local/search/keyword.json?page=1&size=15&sort=accuracy&query=%EA%B0%95%EB%82%A8%EC%97%AD" \
    	-H "Authorization: KakaoAK {REST_API_KEY}"
    ```
<br>