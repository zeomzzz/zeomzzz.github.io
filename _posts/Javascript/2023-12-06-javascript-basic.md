---
title:  "Javascript 기초 문법"
excerpt: ""

categories:
  - Javascript
tags:
  - [Javascript]

toc: true
toc_sticky: true
 
date: 2023-12-06
last_modified_at: 2023-12-06
---

<br>

# **1. Hello World**

## **HTML vs. CSS vs. Javascript**

| 언어 | 역할 |
| --- | --- |
| HTML 5 | - 요소들의 배치와 내용을 기술 |
| CSS3 | - 색, 크기, 애니메이션 등을 정의 |
| Javascript | - 웹사이트를 실질적으로 움직이게 하는 역할

- Javascript Engine이 실행
- Javascript Engine은 웹브라우저(Safari, Firefox, Chrome, Edge, Opera 등)에 포함되어 있음 ex. V8
- Javascript의 실행환경(Runtime) : Javascript Engine이 포함된 웹브라우저

<br>

## **Javascript의 특징**

- 동적 타입 언어
    - 하나의 변수에 다양한 타입의 자료형을 넣을 수 있음
    - 유연하지만 에러가 발생할 확률도 있음
- 호이스팅
    - 코드를 실행하기 전에 변수선언/함수선언을 해당 스코프의 최상단으로 끌어올려진 것 같은 현상
    - 대상 : 함수선언식

<br>
<br>

# **2. 변수와 상수**

## **변수 선언하기**

```jsx
let age = 25;
age = 30;

var height = 160;
height = 180;
```

- `let` vs. `var`
    - `let` : 변수 재선언 불가능
    - `var` : 변수 재선언 가능

<br>
<br>

## **변수 명명 규칙**

1. 기호 사용할 수 없음. 단, _(underscore), $(dollar)는 가능
2. 변수명은 숫자가 아닌 문자로 시작
3. 예약어 피하기 ex. if

<br>
<br>

## **상수 선언하기**

```jsx
const age = 25;
```

- 변수 vs. 상수
    - 상수 : 선언 이후에 값을 변경할 수 없음
        - 선언과 동시에 값을 초기화해야 함

<br>
<br>

# **3. 자료형과 형 변환**

## **자료형**

- 값을 성질에 따라 분류한 것
- 자료형 확인 방법 : `typeof [변수명]`
- 분류
    
    | 자료형 | 설명 |
    | --- | --- |
    | Primitive Type (원시 타입) | - 한 번에 하나의 값만 가질 수 있음 <br> - 하나의 고정된 저장 공간 이용 <br> - number, string, boolean, undefined, null |
| Non-Primitive Type (비 원시 타입) | - 한번에 여러 개의 값을 가질 수 있음 <br> - 여러 개의 고정되지 않은 동적 공간 사용 <br> - object, array, function |

<br>

### **Primitive Type**

<br>

① number

- 정수와 실수를 구분하지 않음
- 사칙연산 가능
- Infinity, -Infinity, NaN을 포함
    - NaN : 수학적인 연산 실패의 결과값

<br>

② string

- `“”` or `‘’` or ````로 감쌀 수 있음
    - ````을 이용하면 `${}`를 이용하여 변수의 값을 넣을 수 있음 (Template Literal)
        
        ```jsx
        let name = "zeomzzz";
        let call = `hello ${name}`;
        console.log(call); // hello zeomzzz
        ```
        
<br>

③ boolean

- 참(true)/거짓(false)만을 저장

<br>

④ null

- 의도적으로 아무것도 담겨있지 않다는 것을 나타낼 때
- Javascript는 null을 실제로 넣어줘야 사용할 수 있음
    
    ```jsx
    let a;
    console.log(a); // undefined
    let b = null;
    console.log(b); // null
    ```
    
<br>

⑤ undefined

- 변수를 선언하고 아무런 값도 할당하지 않았을 때

<br>
<br>

## **형 변환 (casting)**

- 값은 유지하면서 자료형만 변환

<br>

① 묵시적 형변환

- Javascript Engine이 자동으로 형변환을 해주는 것
    
    ```jsx
    let a = 12;
    let b = "2";
    console.log(a * b); // 24 (number)
    console.log(a + b); // 122 (string)
    ```

<br>

② 명시적 형변환

- 프로그래머가 의도적으로 형 변환
    
    ```jsx
    let a = 12;
    let b = "2";
    console.log(a + parseInt(b)); // 14 (number)
    ```

<br>
<br>  

# **4. 연산자**

- 대입연산자(`=`) : 변수에 값을 넣는 역할을 하는 연산자
- 산술연산자(`+`, `*`, `-`, `/`, `%`) : 사칙연산을 할 수 있도록 하는 연산자
- 연결연산자
    - 연결연산 : 두 개 이상의 문자열을 이어 붙이는 연산
        
        ```jsx
        let a = "1";
        let b = "2";
        console.log(a + b); // 12
        ```
        
    - 묵시적 형변환을 포함
- 복합연산자(`+=`, `-=`, `*=`, `/=`) : 산술연산자를 대입연산자와 함께 활용
- 증감연산자(`++`, `--`) : 산술연산자를 두 번 사용하여 원시타입 중 숫자형을 증가시키거나 감소시키는 연산
    - 변수의 뒤(후위연산)나 앞(전위연산)에 붙일 수 있음
- 논리연산자(`!`, `&&`, `||`) : boolean 자료형을 위한 연산자
    - `!` : NOT
    - `&&` : AND. 피연산자 두 개가 모두 참이어야 참
    - `||` : OR. 둘 중 하나만 참이어도 참
- 비교연산자(`==`, `!=`, `===`, `!==`, `>`, `<`, `>=`, `<=`)
    - `==`, `!=` : 값만 비교
    - `===`, `!==` : 값과 자료형 모두 비교
- null 병합 연산자(`??`) : 양쪽의 피연산자 중 null이나 undefined가 아닌 값을 선택
    
    ```jsx
    let a;
    a = a ?? 10;
    console.log(a); // 10
    ```

<br>
<br>

# **5. 조건문**

- 연산의 참/거짓에 따라 다른 연산을 실행

<br>

① `if` ~ `else if` ~ `else`

```jsx
if (a >= 7) {
	console.log("7 이상");
} else if (a >= 5) {
	console.log("5 이상");
} else {
	console.log("5 미만");
}
```

<br>

② `switch` ~ `case`

```jsx
let country = "ko";

switch (country) {
  case "ko":
    console.log("한국");
    break;
  case "cn":
    console.log("중국");
    break;
  case "jp":
    console.log("일본");
    break;
  default:
    console.log("미 분류");
    break;
}
```

- `default` : case가 하나도 맞지 않을 때
- `break`를 이용하여 수행하는 명령을 끊어줘야 함

<br>
<br>

# **6. 함수**

```jsx
// 함수 선언식 (실행 X)
function getArea() {
  let width = 10;
  let height = 20;

  let area = width * height;
  console.log(area);
}

// 함수 호출
getArea(); // 200
```

- 매개변수 이용하기
    
    ```jsx
    function getArea(width, height) {
      let area = width * height;
      console.log(area);
    }
    
    getArea(10, 20); // 200
    ```
    
- 지역변수 : 함수 내부에 선언된 변수
    - 함수 내에 선언된 변수는 함수 밖에서 접근할 수 없음
- 전역변수 : 함수 외부에 선언된 변수
    - 함수 외부에 선언된 변수는 함수 내부에서 접근할 수 있음

<br>
<br>

# **7. 함수표현식 & 화살표함수**

## **함수표현식**

- 무명함수(`function ()`)를 변수에 담아서 사용할 수 있음
    
    ```jsx
    let hello = function () {
      return "Hello World";
    }; // 함수 표현식
    
    console.log(hello); // f hello() {}
    const helloMsg = hello();
    console.log(helloMsg); // Hello World
    ```
    
- 함수표현식 vs. 함수선언식
    - 함수선언식은 호이스팅이 가능함
        
        ```jsx
        console.log(helloB()); // 함수선언식 hello
        console.log(helloA()); // TypeError: helloA is not a function
        
        let helloA = function () {
          return "함수표현식 hello";
        };
        
        function helloB() {
          return "함수선언식 hello";
        }
        ```

<br>
<br>        

## **화살표함수**

- 아래 세 가지는 모두 출력이 동일함
    
    ```jsx
    let helloA = function () {
      return "Hello World!";
    };
    
    let helloB = () => {
      return "Hello World!";
    };
    
    let helloC = () => "Hello World!";
    
    console.log(helloA()); // Hello World!
    console.log(helloB()); // Hello World!
    console.log(helloC()); // Hello World!
    ```
    
    - 구현부가 한 줄인 경우에는 중괄호도 생략할 수 있음
- 호이스팅의 대상 X

<br>
<br>

# **8. 콜백함수**

- 다른 함수의 매개변수로 넘겨준 함수

ex. 

```jsx
function checkMood(mood, goodCallback, badCallback) {
  if (mood === "good") {
    goodCallback();
  } else {
    badCallback();
  }
}

function cry() {
  console.log("ACTION :: CRY");
}

function sing() {
  console.log("ACTION :: SING");
}

function dance() {
  console.log("ACTION :: DANCE");
}

checkMood("sad", sing, cry);
```

- checkMood 함수에 매개변수로 goodCallback, badCallback이라는 함수를 넘겨줌

<br>
<br>

# **9. 객체**

- Non-Primitive Type의 자료형에 해당함

<br>

## **객체 만들기**

① 객체 생성자 이용

```jsx
let person = new Object();
```

<br>

② 객체 리터럴 방식

- 중괄호를 이용해서  key-value 쌍(프로퍼티)의 형태로 객체를 만듦
    
    ```jsx
    let person = {
      key1: "value1", // 프로퍼티 (객체 프로퍼티)
      key2: "value2"
    };
    ```
    

- 프로퍼티에 다른 자료형을 넣어도 상관 없음 ex. 함수
- 함수가 아닌 프로퍼티를 **멤버**, 함수인 프로퍼티를 **메서드**라고 함

<br>
<br>

## **객체 프로퍼티에 접근하기**

- 기본 접근 방법
    
    ① 점표기법(`[객체이름].[프로퍼티이름]`)
    
    ```jsx
    person.key1;
    ```
    
    - 없는 프로퍼티에 접근하면 `undefined`
    
    
    ② 괄호표기법 (`[객체이름][["프로퍼티이름"]]`)
    
    ```jsx
    person.["key1"];
    ```
    
    - 반드시 프로퍼티의 key를 문자열 형태로
        - `“”`를 이용하지 않으면 변수로 인식
    - 동적인 파라미터를 전달받는 상황에 유리함
        
        ex. key를 통해서 value를 받아오는 함수가 필요할 때 용이함
        
        ```jsx
        let person = {
          key1: "value1",
          key2: "value2"
        };
        
        console.log(getPropertyValue("key1"));
        
        function getPropertyValue(key) {
          return person[key];
        }
        ```
        

- 프로퍼티에서 다른 프로퍼티에 접근하기
    
    ```jsx
    let person = {
      name: "zeomzzz",
      age: 100,
      say: function () {
        console.log(`Hello ${this["name"]}`);
      }
    };
    
    person.say(); // Hello zeomzzz
    ```
    
    - Template literal 이용
    - this : 객체 자신

- 존재하는 프로퍼티인지 확인 : `[프로퍼티 이름] in [객체 이름]`

<br>
<br>

## **프로퍼티 추가, 수정, 삭제**

```jsx
let person = {
  name: "zeomzzz",
  age: 100
};

// 프로퍼티 추가
// 점표기법 이용
person.location = "한국";
// 괄호표기법 이용
person["gender"] = "Unknown";

console.log(person); // {name: "zeomzzz", age: 100, location: "한국", gender: "Unknown"}

// 프로퍼티 수정
person.name = "zeomz";
person["age"] = 999;
console.log(person); // {name: "zeomz", age: 999, location: "한국", gender: "Unknown"}

// 프로퍼티 삭제
// 1. delete
delete person.age;
delete person["gender"];
console.log(person); // {name: "zeomz", location: "한국"}

// 2. (추천) value를 null 로 변경
person.name = null;
console.log(person); // {name: null, location: "한국"}
```

- 삭제
    - delete : 객체와 프로퍼티 간의 연결 관계를 끊는 것이지 실제로 프로퍼티를 메모리에서 삭제하지는 않음
    - value를 null로 변경 : 삭제한 효과를 내면서도 value를 메모리에서 삭제


>💡 **객체를 const로 선언하였을 때에도 수정, 삭제 가능**
>
>- person이 갖는 Object를 수정하는 것이지, person이라는 상수 자체를 수정하는 행위가 아니기 때문
>    - person이라는 상수 자체를 수정한다 : 대입 연산자를 이용해 새로운 객체를 할당

<br>
<br>

## **그 외의 객체 관련 메서드**

```jsx
let person = {
  name: "zeomzzz",
  age: 100,
  tall: 200
};

// keys to array
const personKeys = Object.keys(person);
console.log(personKeys); // ["name", "age", "tall"]

// values to array
const personValues = Object.values(person);
console.log(personValues); // ["zeomzzz", 100, 200]
```

<br>
<br>

# **10. 배열**

- 비원시 자료형에 해당
- 순서 있는 요소들의 집합

<br>
<br>

## **배열 만들기**

```jsx
let arr1 = new Array(); // 생성자 이용
let arr2 = []; // 배열 리터럴 이용

let arr = [1, 2, 3, 4];
console.log(arr); // [1, 2, 3, 4]
```

- 어느 자료형이든 배열에 들어갈 수 있음
    
    ```jsx
    let arr = [1, "2", true, null, undefined, {}, [], function () {}];
    ```
    
<br>
<br>

## **배열의 값 다루기**

```jsx
let arr = [1, 2, 3, 4, 5];

// 값에 접근 : 인덱스 이용
console.log(arr[0]); // 1
console.log(arr[1]); // 2
console.log(arr[2]); // 3

// 값 추가
arr.push(6); // 맨 뒤에 추가
console.log(arr); // [1, 2, 3, 4, 5, 6]

// 배열의 길이
console.log(arr.length); // 6
```

<br>
<br>

# **11. 반복문**

- 특정 명령을 반복해서 사용할 수 있도록 도와주는 문법

```jsx
const arr = [1, 2, 3, 4];

// for 문
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}

// 객체 순회
let person = {
  name: "zeomzzz",
  age: 100,
  tall: 200
};

for (let i = 0; i < personKeys.length; i++) {
  const curKey = personKeys[i];
  const curValue = person[curKey];

  console.log(`${curKey} : ${curValue}`);
}
```

<br>
<br>

# **12. 배열 내장 함수**

## **forEach**

```jsx
const arr = [1, 2, 3, 4];

arr.forEach((elm) => console.log(elm));
// 1
// 2
// 3
// 4
```

<br>
<br>

## **map**

- 배열의 각 내장함수에 대해서 콜백함수를 한 번 씩 실행하여 배열을 반환
    
    ```jsx
    const arr = [1, 2, 3, 4];
    
    const newArr1 = arr.map((elm) => {
      return elm * 2;
    });
    
    console.log(newArr1); // [2, 4, 6, 8]
    ```

<br>
<br>

## **includes**

- 배열에 값이 존재하는지 확인 (===)
    
    ```jsx
    const arr = [1, 2, 3, 4];
    
    // 배열에 값이 존재하는지 확인 (===)
    let number = 3;
    let string = "3";
    
    console.log(arr.includes(number)); // true
    console.log(arr.includes(string)); // false
    ```

<br>
<br>   

## **indexOf**

- 배열에 값이 존재하면 인덱스를, 없으면 -1을 반환
    
    ```jsx
    const arr = [1, 2, 3, 4];
    
    // 배열에 값이 존재하는지 확인 (===)
    let number = 3;
    let string = "3";
    
    // 베열에 값이 존재할 때의 인덱스 : 없으면 -1
    console.log(arr.indexOf(string)); // -1
    console.log(arr.indexOf(number)); // 2
    ```

<br>
<br>   

## **findIndex**

- 콜백함수가 true를 반환하는 첫번째 요소의 인덱스를 반환
    
    ```jsx
    const arr = [
      { color: "red" },
      { color: "black" },
      { color: "blue" },
      { color: "green" }
    ];
    
    console.log(arr.findIndex((elm) => elm.color === "green")); // 3
    ```
    
<br>
<br>

## **find**

- 콜백함수가 true를 반환하는 첫번째 요소를 반환
    
    ```jsx
    const arr = [
      { color: "red" },
      { color: "black" },
      { color: "blue" },
      { color: "green" }
    ];
    
    console.log(arr.find((elm) => elm.color === "green")); // {color: "green"}
    ```
    
<br>
<br>

## **filter**

- 콜백함수가 true를 반환하는 모든 요소를 배열로 반환
    
    ```jsx
    const arr = [
      { num: 1, color: "red" },
      { num: 2, color: "black" },
      { num: 3, color: "blue" },
      { num: 4, color: "green" },
      { num: 5, color: "blue" }
    ];
    
    console.log(arr.filter((elm) => elm.color === "blue"));
    // [Object, Object]
    // 0: Object
    // num: 3
    // color: "blue"
    // 1: Object
    // num: 5
    // color: "blue"]
    ```
    
<br>
<br>

## **slice(begin, end)**

- 배열에서 인덱스가 begin에서 end-1까지인 요소를 반환
    
    ```jsx
    const arr = [
      { num: 1, color: "red" },
      { num: 2, color: "black" },
      { num: 3, color: "blue" },
      { num: 4, color: "green" },
      { num: 5, color: "blue" }
    ];
    
    console.log(arr.slice(0, 2));
    // [Object, Object]
    // 0: Object
    // num: 1
    // color: "red"
    // 1: Object
    // num: 2
    // color: "black"
    ```

<br>    
<br>

## **concat**

- 배열을 연결하여 하나의 배열을 반환
    
    ```jsx
    const arr1 = [
      { num: 1, color: "red" },
      { num: 2, color: "black" }
    ];
    
    const arr2 = [
      { num: 3, color: "blue" },
      { num: 4, color: "green" },
      { num: 5, color: "blue" }
    ];
    
    console.log(arr1.concat(arr2));
    
    // [Object, Object, Object, Object, Object]
    // 0: Object
    // num: 1
    // color: "red"
    // 1: Object
    // num: 2
    // color: "black"
    // 2: Object
    // num: 3
    // color: "blue"
    // 3: Object
    // num: 4
    // color: "green"
    // 4: Object
    // num: 5
    // color: "blue"
    ```

<br>
<br>

## **sort**

- 문자열을 기준으로 정렬
    
    ```jsx
    let chars = ["가", "라", "다"];
    chars.sort();
    console.log(chars); // ["가", "다", "라"]
    
    let numbers = [0, 2, 30, 4, 25, 1];
    numbers.sort();
    console.log(numbers); // [0, 1, 2, 25, 30, 4]
    ```
    
- 숫자를 정렬할 때는 직접 비교함수를 생성하여 정렬
    
    ```jsx
    let numbers = [0, 2, 30, 4, 25, 1];
    
    const compare = (a, b) => {
      if (a > b) {
        return 1; // a가 뒤로
      }
    
      if (a < b) {
        return -1; // a가 앞으로
      }
    
      return 0; // 자리를 바꾸지 않음
    };
    
    numbers.sort(compare);
    
    console.log(numbers); // [0, 1, 2, 4, 25, 30]
    ```
    
<br>
<br>

## **join**

- 배열 내 요소를 합친 문자열을 반환
    
    ```jsx
    const arr = ["hello", "nice", "to", "meet", "you"];
    
    console.log(arr.join()); // hello,nice,to,meet,you
    console.log(arr.join(" ")); // hello nice to meet you
    ```

<br>
<br>

---

**참고자료**

<br>

- [한입 크기로 잘라 먹는 리액트(React.js) : 기초부터 실전까지](https://inf.run/N9fZn) - 섹션 1

<br>
<br>
