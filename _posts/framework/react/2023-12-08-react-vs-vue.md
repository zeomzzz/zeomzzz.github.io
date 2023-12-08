---
title:  "Vue.js vs. React.js"
excerpt: ""

categories:
  - React.js
tags:
  - [Vue.js, React.js]

toc: true
toc_sticky: true
 
date: 2023-12-08
last_modified_at: 2023-12-08
---

<br>
<div class="descipt-font">
가장 많이 사용하는 프론트엔드 툴 React.js와 Vue.js! <br> 두 가지 툴을 비교해보았다.
</div>
<br>

# **공통점**

- 컴포넌트 기반
    - 컴포넌트(Component) : 유저가 사용하는 시스템에 대한 조작 장치
        
        ex. page, header bar, footer bar, navbars, button, form
        
- Virtual DOM 방식
    - 실제 돔에 접근하여 조작하지 않고, 이를 추상화시킨 자바스크립트 객체를 이용해 사용
    - 따라서 빠른 렌더링이 가능함

- Server Side Rendering
    - 서버에서 페이지를 그려 클라이언트로 보낸 후 화면에 표시함
    - 단점
        - Node.js 환경에서 실행되어 Node.js 웹 애플리케이션 실행 방법을 알아야 함
        - 라이프사이클 훅과는 다른 환경(브라우저가 아닌 Node.js)에서 동작하기 때문에 `beforeCreate`와 `created`에서 `window`나 `document`와 같은 브라우저 객체에 접근할 수 없음

<br>
<br>

# **차이점**

## **1. React는 라이브러리, Vue는 프레임워크이다**

- React는 UI 라이브러리 -> 개발자에 따라 자유롭게 개발 가능
    - 참고가 용이하고, 라이브러리의 일부분만 가져와서 사용할 수 있음
    - 전역 상태 관리, 라우팅, 빌드 시스템 사용 등을 위해 별도의 라이브러리를 사용해야 함 -> 사용자가 필요시에 따라 사용/미사용 할 수 있음
        
        ex. Redux, Recoil, React-router-dom 등
        
- Vue는 프레임워크 -> 프레임워크가 지원하는 방식으로만 개발 가능
    - 부분적이 사용이 불가능하고 프레임워크가 지원하는 문법에 따라 작성해야 함
        
        -> 라이브러리보다 더 많은 기능을 기본적으로 제공
        

ex. button을 조건에 따라 보이고, 보이지 않게 설정하기

```jsx
// 1. React
// && 연산자 방식
<div>
	{isVisible && <button>버튼</button>}
</div>

// 삼항 연산자 방식
<div>
	{isVisible ? <button>버튼</button> : null}
</div>

// 2. Vue (v-if 한 가지 방식)
<div>
	<button v-if="isVisible">버튼</button>
</div>
```

<br>

## **2. 코드 형태의 차이**

- React는 JSX(JavaScript XML) 형태로 작성
    - Javascript 만으로 UI 로직과 DOM을 구현함
    - 자유도가 높음
- Vue는 HTML, JS, CSS 코드 영역을 분리해서 작성
    - `<template>` : HTML
    - `<script>` : JS
    - `<style>` : CSS
    - 자유도가 낮음

<br>

## **3. 컴포넌트 분리와 재사용 - React는 가능, Vue는 불가능**

- React는 컴포넌트 분리와 재사용이 가능
    - 파일 별로 컴포넌트를 분리하고, 새로운 함수형 컴포넌트를 생산하고, 이를 props 형태로 전달하거나 다른 속에서 재사용하는 것이 용이함
- Vue는 컴포넌트 분리와 재사용이 불가능
    - 컴포넌트 분리를 위해 새로운 파일을 만들어야 함
    - 이에 따라 하나의 파일에 `<template>`, `<script>`, `<style>`을 모두 작성해야 함
    - props를 전달할 때에도 해당 컴포넌트를 사용하는 모든 파일을 오가며 작성해야 함

<br>

## **4. 진입 장벽 / 러닝 커브**

- React > Vue
    - React가 자유도가 높고, 사용 가능한 미들웨어가 다양해서 더 많은 학습이 필요
    - Vue는 상태 관리를 위한 라이브러리가 다양하지 않고 단순화되어있어 진입 장벽이 낮음

<br>

## **5. 속도**

- 미세한 차이이지만 Vue > React
    - 속도 이슈에 민감한 경우에 Vue를 사용하기도 함

<br>

## **6. 그 외**

- React
    - 장점
        - 자유도
        - Typescript 사용 가능
        - 변화에 빠름
        - 월등한 사용량
        - 변화가 빠름
    - 단점
        - 자유도가 높아서 개발자마다 코딩 스타일 통일하기 위해 더 많은 커뮤니케이션 필요
        - 모든 것이 javascript여서 js에 대한 숙련도 필요
            - html 역할을 하는 jsx도 js의 확장된 문법, css도 css-in-js로 작성해야 함
- Vue
    - 장점
        - 러닝 커브가 낮음

<br>

## **7. 언제 어떤 툴을 사용할까?**

- React
    - 프로젝트의 규모가 크거나 점점 확장되는 경우
    - javascript 문법에 능숙한 경우
    - 컴포넌트를 나누어 UI 재사용하고 싶은 경우
    - Typescript를 이용하려는 경우
    - 웹 개발, 앱 개발을 모두 하고 싶은 경우 (React Native)
- Vue
    - 규모가 작거나 가벼운 프로그램을 만들려는 경우
    - 속도 이슈에 민감한 경우
    - javascript 문법에 미숙한 경우
    - 기존의 html, css, js 구조로 작성한 코드를 SPA로 옮기려는 경우
    - 개발자간의 코드 통일 비용을 낮추고 싶은 경우

<br>

---

**참고 자료**

<br>

- [리액트(React.js) vs 뷰(Vue.js)](https://nyol.tistory.com/148)
- [React vs Vue 장단점 비교](https://velog.io/@leehaeun0/React-vs-Vue-%EC%9E%A5%EB%8B%A8%EC%A0%90-%EB%B9%84%EA%B5%90)
- [[Vue] Vue & React의 차이와 장단점](https://koras02.tistory.com/218)
- [서버 사이드 렌더링이란?](https://joshua1988.github.io/vue-camp/nuxt/ssr.html#%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%90%E1%85%B3-%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B3-%E1%84%85%E1%85%A6%E1%86%AB%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)

<br>