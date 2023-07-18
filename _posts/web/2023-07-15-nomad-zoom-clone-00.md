---
title:  "[NomadCoders Zoom Clone] 00. Introduction"
excerpt: ""

categories:
  - Web
tags:
  - [Web, Lecture, WebSocket]

toc: true
toc_sticky: true
 
date: 2023-07-15
last_modified_at: 2023-07-15
---

<br>

# **00. INTRODUCTION**

<br>

## **0.1. Requirements**

<br>

## **0.2 Server Setup**

<br>

- 폴더를 만들고 npm i
- package.json에서 main, scripts, keywords, author 삭제. license는 MIT license

<br>
<br>

### **Nodemon 설치**

<br>

- Nodemon : 프로젝트를 살펴보고 변경 사항이 있을 시에 서버를 재시작해주는 프로그램
    
    ```javascript
    npm i nodemon -D
    ```
    
- nodemon.json 생성
    
    ```jsx
    nodemon.json
    {
        "exec": "babel-node src/server.js" // src/server.js만 실행
    }
    ```

<br>
<br>

### **Babel 설치**

<br>

```javascript
npm i @babel/core @babel/cli @babel/node -D
```

- babel.config.json 생성
    - babel.config.json에 presets 설정 및 설치
        
        ```jsx
        // babel.config.json
        
        {
            "presets": ["@babel/preset-env"]
        }
        ```
        
        ```python
        npm i @babel/preset-env -D
        ```

<br>
<br>

### **server.js 생성**

<br>

- src 폴더 생성하고 그 하위에 생성
- server.js가 서버가 됨

<br>
<br>

### **git ignore 생성**

<br>

```javascript
touch .gitignore
```

- node_modules를 gitignore
    
    ```jsx
    // .gitignore
    /node_modules
    ```
    
<br>
<br>

### **package.json**

<br>

- package.json 에 scripts 추가 : dev가 nodemon만 호출
    
    ```jsx
    {
      "name": "zoom",
      "version": "1.0.0",
      "description": "Zoom Clone using WebRTC and Websockets",
      "license": "MIT",
      "scripts": {
        "dev": "nodemon"
      },
      "devDependencies": {
        "@babel/cli": "^7.22.9",
        "@babel/core": "^7.22.9",
        "@babel/node": "^7.22.6",
        "@babel/preset-env": "^7.22.9",
        "nodemon": "^3.0.1"
      }
    }
    ```
    
    - nodemon이 호출되면 nodemon.json을 살펴보고 거기에 있는 코드를 실행

<br>
<br>

### **express 설치**

<br>

```jsx
npm i express
```

<br>
<br>

### **pug 설치**

<br>

```jsx
npm i pug
```

<br>
<br>

### **server.js 에 express 설정**

<br>

```jsx
import express from "express"

const app = express();

console.log("hello");

app.listen(3000); // 포트 3000을 listen
```

- 실행
    
    ```jsx
    npm run dev
    ```
    
<br>
<br>

## **0.3 Frontend Setup**

<br>

### **페이지 렌더 확인**

<br>

- src/public/views/home.pug
    
    ```jsx
    doctype html
    html(lang="en")
        head
            meta(charset="UTF-8")
            meta(http-equiv="X-UA-Compatible", content="IE=edge")
            meta(name="viewport", content="width=device-width, initial-scale=1.0")
            title Zoomzzz
        body 
            h1 It works!
    				script(src="/public/js/app.js") // script 추가
    ```
    
- server.js
    
    ```jsx
    import express from "express"
    
    const app = express();
    
    app.set("view engine", "pug"); // view engine을 pug로 설정
    app.set("views", __dirname + "/views"); // /views로 디렉토리 설정
    
    app.get("/", (req, res) => res.render("home"));
    
    const handleListen = () => console.log(`Listening on http://localhost:3000`);
    
    app.listen(3000, handleListen); // 포트 3000을 listen
    ```
    
- 서버 실행하고 localhost 3000 접속하면 “It works!” 보임  == 페이지를 render 하고 있음

<br>
<br>

### **Express에서 static 작업**

<br>

- server.js에 유저가 /public으로 갔을 때 dirname + “/public”을 보여주도록 작업
    
    ```jsx
    import express from "express"
    
    const app = express();
    
    app.set("view engine", "pug");
    app.set("views", __dirname + "/views");
    app.use("/public", express.static(__dirname + "/public")); // **유저가 /public으로 가면 dirname + "/public"을 보여준다. (template 위치)
    app.get("/", (req, res) => res.render("home"));
    
    const handleListen = () => console.log(`Listening on http://localhost:3000`);
    
    app.listen(3000, handleListen); // 포트 3000을 listen
    ```
    
- app.js : 유저에게 보여지는 Frontend 화면
- nodemon이 /public 폴더 무시하도록 수정 : `"ignore": ["src/public/*"]`
    
    ```jsx
    // nodemon.json
    
    {
        "ignore": ["src/public/*"], // **
        "exec": "babel-node src/server.js"
    }
    ```
    
    - Frontend를 수정할 때마다 nodemon이 재시작되지 않고, view나 서버를 수정할 때만 재시작

→ babel-node를 실행시키면 babel-node는 바로 babel.config.json을 찾고, 거기에서 코드에 적용되어야 하는 preset을 실행

<br>
<br>

### **[MVP CSS](https://andybrewer.github.io/mvp/)**

<br>

- 프로젝트에 포함시킬 수 있는 css 파일
- 기본 HTML tag를 예쁘게 바꿔줌
- home.pug의 head에 추가 : `link(rel="stylesheet", href="https://unpkg.com/mvp.css")`

<br>
<br>