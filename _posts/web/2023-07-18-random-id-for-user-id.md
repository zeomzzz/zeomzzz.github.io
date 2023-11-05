---
title:  "사용자에게 랜덤ID 부여하기 위한 난수 생성 (Java)"
excerpt: ""

categories:
  - Web
tags:
  - [Web]

toc: true
toc_sticky: true
 
date: 2023-07-18
last_modified_at: 2023-07-18
---

<br>

## **Java로 난수 생성**

<br>

### **Math.random**

<br>

- `Math.random` : 0.0 이상 1.0 미만의 랜덤 값 반환
- `(int)(Math.*random*() * 100) + 1` : 1 이상 100 이하의 정수 반환

<br>
<br>

**최대값, 최소값 설정**

<br>

- 최대값 설정 : `Math.random() * 최대값`
- 최소값 설정 : `Math.random() + 최소값`
- 최대값 및 최소값 설정 : `(Math.random() * (최대값 - 최소값)) + 최소값`

<br>
<br>

**12자리 난수 생성하려면?**

<br>

: `(long)((Math.*random*() * (Math.*pow*(10, 11) - 11111111111L)) + Math.*pow*(10, 11))`

<br>
<br>

### **java.util.Random**

<br>

- nextInt() : 32bit로 표현 가능한 Int형의 난수 생성
    
    ```java
    Random random = new Random();
    System.out.println(random.nextInt());
    ```
    
- 이 외의 자료형의 난수 생성
    
    
    | 자료형 | 상세 |
    | --- | --- |
    | nextDouble() | 0과 1 사이의 Double 난수 |
    | nextFloat() | 0과 1 사이의 Float 난수 |
    | nextLong() | Long 타입의 난수 |
    | nextBoolean() | True or False 무작위로 |

<br>
<br>

**최대값 설정**

<br>

ex. 100 미만의 정수 난수 생성

```java
int bound = 100;
		
Random random = new Random();
System.out.println(random.nextInt(bound));
```

<br>

>💡 **Random의 초기값(seed)?**
>
>- Random 클래스의 생성자는 인자로 seed를 받고, 동일한 seed를 입력하면 항상 동일한 난수를 생성
>
>```java
>Random random1 = new Random(10);
>Random random2 = new Random(10);
>		
>System.out.println(random1.nextInt()); // -1157793070
>System.out.println(random2.nextInt()); // -1157793070
>```

<br>
<br>

### **Apache commons-math3 라이브러리**

<br>

- Gradle 프로젝트의 build.gradle에 dependency cnrk
    
    ```java
    dependencies {
      compile group: 'org.apache.commons', name: 'commons-math3', version: '3.6.1'
      ...
    }
    ```
    
- 난수 생성
    
    ```java
    int randomInt = new RandomDataGenerator().getRandomGenerator().nextInt();
    ```
    
    - Double은 `nextDouble()`, Long은 `nextLong()` …
- 범위 지정하여 난수 생성
    
    ```java
    int leftLimit = 100;
    int rightLimit = 120;
    int randomInt = new RandomDataGenerator().nextInt(leftLimit, rightLimit);
    ```

<br>
<br>

### **그런데,, DB에서 난수 중복 체크..?**

<br>

방법 1. 미리 난수를 DB에 저장하고 빼서 쓰기

방법 2. 시간 + 아이피 또는 php 세션값을 조합 (중복 확률 낮음)

<br>

---

**참고 자료**

- [Java - Random number(난수) 생성하는 방법](https://codechacha.com/ko/java-generate-random-number/#apache-commons-math3-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-%EB%82%9C%EC%88%98-%EC%83%9D%EC%84%B1)
- [프로그래밍 하시는 분들께 질문 하나 드릴게요 ..](https://www.2cpu.co.kr/bbs/board.php?bo_table=QnA&wr_id=518055)

<br>