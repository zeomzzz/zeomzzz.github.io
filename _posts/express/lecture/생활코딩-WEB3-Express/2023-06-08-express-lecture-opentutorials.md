---
title:  "WEB3 - Express 생활코딩"
excerpt: "[WEB3 - Express 생활코딩] 듣는 중"

categories:
  - Express.js
tags:
  - [Framework, Lecture, Express.js]

toc: true
toc_sticky: true
 
date: 2023-06-08
last_modified_at: 2023-06-08
---

<br>

# **01강. 수업 소개, 02강. 실습준비**

<br>

### **Web Framework**

<br>

- 반복적으로 등장하는 일들을 자동화
- Express : node.js Framework

<br>
<br>

### **사전 확인 사항**

<br>

- 사전 확인 사항 : [Node.js 수업 최종 코드](https://github.com/web-n/Nodejs)
- 실행방법 : `pm2 start main.js --watch`
  - `npm install` 로 node modules 설치 후 실행

<br>
<br>

# **03강-1. Hello World 1**

<br>

- npm express 설치 : `npm install express --save`
- [express 사이트](https://expressjs.com/)
- Hello World 코드
    
    ```java
    const express = require('express')
    const app = express()
    const port = 3000
    
    app.get('/', (req, res) => {
      res.send('Hello World!')
    })
    
    app.listen(port, () => {
      console.log(`Example app listening on port ${port}`)
    })
    ```
    
- [http://localhost:3000/](http://localhost:3000/) 에서 ‘Hello World!’ 출력 확인

<br>

# **03강-2. Hello World 2**

<br>

- Hello World 코드 풀이
    
    ```jsx
    var express = require('express') // express 모듈을 express라는 이름으로 load
    var app = express() // express를 함수처럼 호출(-> express는 함수!) resturn된 값을 app에 넣음. app은 Application 객체
    const port = 3000
    
    // get : route, routing을 함. 방향을 잡는 것. path마다 적당한 응답을 줌
    // app.get('/', (req, res) => {
    //   res.send('Hello World!')
    // })
    app.get('/', function(req, res) {
      return res.send('/')
    });
    // http://localhost:3000/ 실행 결과 : /
    
    app.get('/page', function(req, res) {
      return res.send('/page')
    });
    // http://localhost:3000/page 실행 결과 : /page
    // -> app.get의 첫번째 인자로 path를 전달하여 routing
    // (이전에는 pathname== ~ 로 구현)
    
    app.listen(port, () => { // listen에 3000을 주면, listen이 실행될 때 웹서버가 실행됨
      console.log(`Example app listening on port ${port}`) // listen에 성공하면 실행
    })
    // 기존 코드 중 app.listen(3000); 과 동일
    ```
    
<br>

# **04강. 홈페이지 구현**