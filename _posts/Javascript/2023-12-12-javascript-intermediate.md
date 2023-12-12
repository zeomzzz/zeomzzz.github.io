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
last_modified_at: 2023-12-12
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

---

**참고자료**

<br>

- [한입 크기로 잘라 먹는 리액트(React.js) : 기초부터 실전까지](https://inf.run/N9fZn) - 섹션 2

<br>
<br>
