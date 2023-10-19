---
title:  "Java Style Guide - Google Java Style Guide"
excerpt: ""

categories:
  - Java
tags:
  - [Style Guide]

toc: true
toc_sticky: true
 
date: 2023-10-19
last_modified_at: 2023-10-19
---

<br>

# **Google Java Style Guide**

## **3. 소스 파일 구조**

1. 라이센스/저작권 정보

2. 패키지(Package) 구문
- 줄바꿈 X

3. 임포트(Import) 구문
- static과 non-static 둘 다 있다면, 개행을 하고 두 개의 블럭으로 나눔 (그 외에는 개행 X)
- 각각의 블럭은 ASCII 순서대로 정렬 (참고 : ‘`.`’가 ‘`;`’ 전에 옴)
- 클래스에는 static 임포트 X

4. 정확하게 하나의 최상위 클래스
- 논리적 순서에 따라 작성
- 오버로드 코드는 가운데 다른 코드들 없이 차례로 작성

<br>
<br>

## **4. 포매팅**

1. 괄호
- 선택사항에서도 사용됨 ex. `if`, `else`, `for`, `do`, `while`
- 비어있지 않은 블럭 : K & R 스타일 (단, Enum 클래스에는 예외)
    - 여는 괄호 앞에는 줄 바꿈 X
    - 여는 괄호 다음에 줄 바꿈
    - 닫는 괄호 전에 줄 바꿈
    - 닫는 괄호 다음에 줄 바꿈은 구문, 메소드, 생성자, 클래스가 끝났을 때
    
    ex.
    
    ```java
    return () -> {
      while (condition()) {
        method();
      }
    };
    
    return new MyClass() {
      @Override public void method() {
        if (condition()) {
          try {
            something();
          } catch (ProblemException e) {
            recover();
          }
        } else if (otherCondition()) {
          somethingElse();
        } else {
          lastThing();
        }
      }
    };
    ```
    
- 멀티 블럭에서는 괄호를 열자마자 끝내지 않음
    
    ex. 
    
    ```java
    try {
    	doSomething();
    } catch (Exception e) {}
    ```
    

2. 들여쓰기 : +2 스페이스 (🚨우테코 : +4 스페이스)

3. 줄 당 하나 서술

4. 열 제한 : 100개의 문자 (🚨우테코 : 120개 문자)
- 아래 예외 제외하고 이 숫자를 넘기면 줄 바꿈
    - 예외
        - 개행이 불가능한 경우
        - package나 import 구문들
        - 쉘에 복사 붙여넣기 되는 커멘드 라인에 대한 주석

5. 줄 바꿈 : +4 스페이스 (🚨우테코 : +8 스페이스)
- 원래 줄보다 +4 스페이스 들여쓰기
- 여러 줄 바꿈이 있을 때, 들여쓰기는 +4 이상으로 변동 가능

6. 공백
- 수직 공백
    - 연속적인 멤버나 클래스의 초기화: 필드, 생성자, 메소드, 중첩 클래스, 정적 초기화 그리고 인스턴스 초기화
        - 예외: 두 개의 연속된 필드의 공백(선택), enum 상수의 컴마 다음 개행(선택)
    - 다수의 공백 줄은 지양
- 수평 공백
    - 예약어를 나누는 경우
        - if, for, catch 같은 예약어 이후 나오는 여는 괄호
        - else, catch 같은 예약어 이후 나오는 닫는 중괄호
    - 중괄호를 여는 모든 경우
        - 예외 두 가지 : `@SomeAnnotation({a, b})`, `String[][] x = {{"foo"}};`
    - 임의의 이항 또는 삼항 연산자 양쪽
    - `,` `:` `;` 혹은 캐스팅 할때 닫는 괄호 뒤에서 사용
    - `//` 더블 슬래시에서 양쪽 : 여러 개의 공백도 허용
    - 변수, 배열(선택)의 선언문 사이
    - 타입 애너테이션이나 대괄호

7. 소괄호 그룹 : 추천

8. 이외 특별한 구조
- enum : 각 컴마 다음의 개행은 선택
- 변수 선언
    - 한 번에 하나의 변수만 초기화 ex. `int a, b;` : X
    - 단, for loop 헤더에서는 여러 변수 선언 가능
    - 지역변수는 범위를 좁히기 위해 처음 사용될 때 가까운 위치에 선언(이유가 있어야 함)
- 배열
    - 초기화 : block-like construct 처럼도 가능
    - 단, C 스타일 배열 선언문은 금지 ex. `String[] args` : O, `String args[]` : X
- Switch 구문
    - switch 라벨 이후 개행
- 애노테이션
    - documentation 블럭 바로 이후에 클래스, 함수, 생성자에 적용
    - 한 줄에 하나의 애노테이션
        - 단, 파라미터가 없는 단일 애노테이션, 필드에 적용되는 애노테이션들은 한 줄에 쓸 수 있음
- 숫자 리터럴 : long의 값을 가지는 정수 리터럴은 대문자 L의 접미사 (소문자면 숫자 1과 헷갈릴 수 있음)

<br>
<br>

## **5. 네이밍**


1. 공통
- 식별자는 ASCII 숫자, 문자만 쓰고 일부 경우 언더 스코어 사용 (-> 유효한 식별자 이름은 정규직 \w+ 와 매칭됨)
- 패키지명 : 전부 소문자로 서로 붙여서 연속된 단어 (언더스코어도 X)
- 클래스 이름 : UpperCamelCase. 주로 명사구이고 가끔은 형용사나 형용사구
- 함수 이름 : lowerCamelCase. 동사 혹은 동사구
    - 언더스코어는 JUnit 테스트에서 논리적 컴포넌트를 분리시키기 위해 각각을 lowerCamelCase로 변경시켜 나올 수 있음 ex. _
- 상수 이름 : CONSTANT_CASE(모두 대문자이고 각 단어는 하나의 언더스코어로 구분)
    - 상수 : static final 필드. 변경 X
- 상수가 아닌 필드의 이름 (ex. static) : lowerCamelCase. 명사나 명사구
- 파라미터 이름 : lowerCamelCase
- 지역변수 이름 : lowerCamelCase
- 타입 변수의 이름 : 하나의 대문자 혹은 뒤에 하나의 숫자가 따라오거나 클래스에 사용되는 이름의 형식에 대문자 T가 붙음

<br>
<br>

## **6. 프로그래밍 연습**

1. @Override : 항상 사용
2. 예외 catch : 생략 X
3. 정적(static) 멤버 : 클래스 사용할 수 있음
    
    ```java
    Foo aFoo = ...;
    Foo.aStaticMethod(); // good
    aFoo.aStaticMethod(); // bad
    somethingThatYieldsAFoo().aStaticMethod(); // very bad
    ```

<br>
<br>

## **7. Javadoc**

1. 포매팅
- 일반 형식
    
    ```java
    /**
     * Multiple lines of Javadoc text are written here,
     * wrapped normally...
     */
    
    // 또는
    
    /** An especially short bit of Javadoc. */
    ```
    

- 문단
- 블럭 태그
    - 종류 : `@param`, `@return`, `@throws`, `@deprecated`
    - 빈 서술이 있으면 안됨

2. 요약 조각
- Javadoc 블록의 시작
- 완전한 명사가 아닌 명사구 또는 동사구
    
    ex. `/** @return the customer ID */`* : X, *`/** Returns the customer ID. */` : O
    

3. 사용 위치
- 매 public class. 이 안의 public, protected 멤버
- 예외 : 자가-설명 메소드, 오버라이드
- 다른 클래스 및 멤버는 필요에 따라 또는 원하는대로

<br>

---

**참고 문서**

<br>

- [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
- [woowacourse-docs의 Java Style Guide](https://github.com/woowacourse/woowacourse-docs/blob/main/styleguide/java/README.md)
- [[JAVA] Google Java Style Guide 번역](https://newwisdom.tistory.com/m/96)
- [Google Java Style Guide(JunHoPark93)](https://github.com/JunHoPark93/google-java-styleguide#google-java-style-guide)


<br>
<br>