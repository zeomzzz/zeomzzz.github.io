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
last_modified_at: 2023-12-31
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

# **3. 단락회로 평가**

<br>

- 단락회로 평가 : 왼쪽에서 오른쪽으로 연산하는 논리연산자의 연산 순서를 이용
    - 논리연산자 : `&&`, `||`, `!`
    
    ex. 
    
    ```java
    console.log(false && true); // false
    console.log(true || true); // true
    ```
    
    - 논리연산자의 오른쪽까지 확인하지 않아도 됨
- truthy와 falsy를 이용한 단락회로 평가
    
    ```java
    const getName = (person) => {
      if (!person) {
        return "객체가 아님";
      }
      return person.name;
    };
    
    let person;
    const name = getName(person);
    console.log(name); // 객체가 아님
    ```
    
    ```java
    const getName = (person) => {
      const name = person && person && person.name; // person : truthy, person.name : truthy. 따라서 person.name을 반환 -> zeomzzz
      return name || "객체가 아님"; // name : truthy -> 뒤를 확인하지 않고 전달
    };
    
    let person = { name: "zeomzzz" };
    const name = getName(person);
    console.log(name); // zeomzzz
    ```

<br>
<br> 

# **4. 비 구조화 할당**

<br>

- 배열의 요소를 순서대로 변수에 쉽게 할당할 수 있는 방법
- 구조분해 할당 이라고도 함

<br>
<br>

## **배열의 비 구조화 할당**

<br>

- 배열의 기본변수 비 구조화 할당 : 대괄호를 이용해서 배열의 값을 순서대로 할당
    
    ```jsx
    let arr = ["one", "two", "three"];
    
    let one1 = arr[0];
    let two1 = arr[1];
    let three1 = arr[2];
    console.log(one1, two1, three1); // one two three
    
    let [one2, two2, three2] = arr;
    console.log(one2, two2, three2); // one two three
    ```
    
- 배열의 선언 분리 비 구조화 할당 : 배열을 선언할 때부터 비 구조화 할당
    
    ```jsx
    let [one3, two3, three3] = ["one", "two", "three"];
    console.log(one3, two3, three3); // one two three
    ```
    
- 변수에 값이 할당되지 못하면 `undefined`
    
    ```jsx
    let [one, two, three, four] = ["one", "two", "three"];
    console.log(one, two, three, four); // one two three undefined
    ```
    
    - 이런 경우에 대비하여 기본값 할당을 할 수 있음
        
        ```jsx
        let [one, two, three, four = "four"] = ["one", "two", "three"];
        console.log(one, two, three, four); // one two three four
        
        let [one, two, three, four = "four"] = ["one", "two", "three", "five"];
        console.log(one, two, three, four); // one two three five
        ```
        
- 비구조화 할당 활용
    - Swap
        
        ```jsx
        let a = 10;
        let b = 20;
        
        [a, b] = [b, a];
        console.log(a, b); // 20 10
        ```
        

<br>
<br>

## **객체의 비 구조화 할당**

<br>

- 객체의 key 값과 일치하는 변수에 property를 할당
    - index가 아닌 key 값을 기준으로 할당!!!
    
    ```jsx
    let object = { one: "one", two: "two", three: "three" };
    
    let one = object.one;
    let two = object.two;
    let three = object.three;
    
    console.log(one, two, three); // one two three
    ```
    
    ```jsx
    let object = { one: "one", two: "two", three: "three" };
    
    let { one, two, three } = object;
    
    console.log(one, two, three); // one two three
    ```
    
- key 값이 아닌 다른 값을 변수명으로 사용하기 : `:` 사용
    
    ```jsx
    let object = { name: "zeomzzz", one: "one", two: "two", three: "three" };
    
    let { name: myName, one, two, three } = object;
    
    console.log(myName, one, two, three); // zeomzzz one two three
    ```
    
- 기본값 지정하기 : `=` 사용
    
    ```jsx
    let object = { name: "zeomzzz", one: "one", two: "two", three: "three" };
    
    let { name: myName, age = 100, one, two, three } = object;
    
    console.log(myName, age, one, two, three); // zeomzzz 100 one two three
    ```

<br>
<br>  

# **5. Spread 연산자**

<br>

- Spread 연산자 : `…`
    - 객체 또는 배열의 값을 새로운 객체에 펼쳐주는 역할
    
    ex. 객체
    
    ```jsx
    // cookie의 base, madeIn이 다른 객체들에 중복됨
    const cookie = {
      base: "cookie",
      madeIn: "korea"
    };
    
    const chocoCookie = {
      base: "cookie",
      madeIn: "korea",
      topping: "choco"
    };
    
    const blueCookie = {
      base: "cookie",
      madeIn: "korea",
      topping: "blueberry"
    };
    
    const strawCookie = {
      base: "cookie",
      madeIn: "korea",
      topping: "strawberry"
    };
    ```
    
    ```jsx
    // Spread 연산자 이용하여 중복을 줄임
    const cookie = {
      base: "cookie",
      madeIn: "korea"
    };
    
    const chocoCookie = {
      ...cookie,
      topping: "choco"
    };
    
    const blueCookie = {
      ...cookie,
      topping: "blueberry"
    };
    
    const strawCookie = {
      ...cookie,
      topping: "strawberry"
    };
    
    console.log(chocoCookie); // {base: "cookie", madeIn: "korea", topping: "choco"}
    ```
    
    ex. 배열
    
    ```jsx
    const noToppingCookies = ["촉촉쿠키", "안촉촉쿠키"];
    const toppingCookies = ["바나나쿠키", "블루베리쿠키", "딸기쿠키", "초코칩쿠키"];
    
    const allCookies = [...noToppingCookies, ...toppingCookies];
    console.log(allCookies); // ["촉촉쿠키", "안촉촉쿠키", "바나나쿠키", "블루베리쿠키", "딸기쿠키", "초코칩쿠키"]
    ```

<br>
<br>

---

**참고자료**

<br>

- [한입 크기로 잘라 먹는 리액트(React.js) : 기초부터 실전까지](https://inf.run/N9fZn) - 섹션 2

<br>
<br>
