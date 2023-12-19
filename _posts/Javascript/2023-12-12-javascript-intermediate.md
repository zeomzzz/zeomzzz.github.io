---
title:  "Javascript 중급 문법"
excerpt: ""

categories:
  - Javascript
tags:
  - [Javascript]

toc: true
toc_sticky: true
 
date: 2023-12-12
last_modified_at: 2023-12-19
---

<br>

# **1. Truthy & Falsy**

<br>

| 분류 | 설명 |
| --- | --- |
| Truthy | - true가 아니어도 true로 분류하는 값 <br> ex. [](빈 배열), {}(빈 중괄호(객체 리터럴)), 숫자값, 문자열, Infinity |
| Falsy | - false가 아니어도 false로 분류하는 값 <br> ex. null, undefined, 0(숫자), -0, NaN, “”(빈 문자열) |

<br>

- falsy를 활용하여 예외 처리를 할 수 있음
    
    ```jsx
    const getName = (person) => {
      if (person === undefined || person === null) {
        return "객체가 아닙니다.";
      }
      return person.name;
    };
    
    let person;
    
    const name = getName(person);
    console.log(name); // 객체가 아닙니다.
    
    // falsy를 이용하여 예외처리
    const getName = (person) => {
      if (!person) {
        return "객체가 아닙니다.";
      }
      return person.name;
    };
    
    let person;
    
    const name = getName(person);
    console.log(name);
    ```

<br>
<br>

# **2. 삼항 연산자**

<br>

- 삼항 연산자를 이용해서 조건식을 간단하게 작성할 수 있음
- 형식 : `[조건] ? [참일 때 수행] : [거짓일 때 수행]`
    
    ```java
    let a = 3;
    a >= 0 ? console.log("양수") : console.log("음수"); // 양수
    ```
    
    ```java
    let a = [];
    
    if (a.length === 0) {
      console.log("빈 배열");
    } else {
      console.log("안 빈 배열");
    } // 안 빈 배열
    
    a.length === 0 ? console.log("빈 배열") : console.log("안 빈 배열"); // 안 빈 배열
    ```
    
- 변수에 삼항연산자의 결과를 저장해서 사용할 수 있음
    
    ```java
    let a = [];
    
    const arrayStatus = a.length === 0 ? "빈 배열" : "안 빈 배열";
    console.log(arrayStatus);
    ```
    
- truthy와 falsy 이용하기
    
    ```java
    let a;
    
    const result = a ? true : false;
    console.log(result); // true
    
    let a = [];
    
    const result = a ? true : false;
    console.log(result); // false
    ```
    
- 삼항연산자 중첩해서 사용하기
    
    ```java
    let score = 100;
    
    score >= 90
      ? console.log("A+")
      : score >= 50
      ? console.log("B+")
      : console.log("F");
    ```
    
    - 가독성이 좋지 않음 -> If문으로 작성 권장

<br>
<br>

---

**참고자료**

<br>

- [한입 크기로 잘라 먹는 리액트(React.js) : 기초부터 실전까지](https://inf.run/N9fZn) - 섹션 2

<br>
<br>
