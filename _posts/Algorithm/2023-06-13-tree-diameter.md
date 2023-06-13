---
title:  "트리의 지름 구하기 증명"
excerpt: ""

categories:
  - Algorithm
tags:
  - [Algorithm, Graph, Tree]

toc: true
toc_sticky: true
 
date: 2023-06-13
last_modified_at: 2023-06-13
---

<br>

*[https://stillloading.tistory.com/108](https://stillloading.tistory.com/108) 와 동일 내용입니다.*

<br>

-   트리의 지름 : 트리에서 가장 먼 두 정점 사이의 거리
-   구하는 방법 : 임의의 점에서 가장 먼 점 A와 A에서 가장 먼 점인 B를 연결하는 점까지의 거리가 트리의 지름
-   관련 문제 : [BOJ 1967 트리의 지름](https://www.acmicpc.net/problem/1967)

<br>
<br>


# **증명**

<br>

- 가정 : 트리의 정점 u와 정점 v를 연결하는 경로가 트리의 지름
- 임의의 정점 x에서 가장 먼 정점이 y 일 때, 아래와 같은 세 가지로 경우가 있음
  - **case 1. x가 u 또는 v**
  - **case 2. y가 u 또는 v**
  - **case 3. x, y, u, v가 서로 다름**

<br>

- 이 중 case 1, 2는 트리의 지름을 구한다는 점이 자명함

<br>
<br>

## **case 3. x, y, u, v가 서로 다른 경우 증명**

<br>

- case 3의 경우 두 가지
  1. **case 3-1. x와 y를 연결하는 경로가 u와 v를 연결하는 경로와 한 점 이상 공유**
  2. **case 3-2. x와 y를 연결하는 경로가 u와 v를 연결하는 경로와 독립**

<br>
<br>

###   **case 3-1. x와 y를 연결하는 경로가 u와 v를 연결하는 경로와 한 점 이상 공유**

<br>

- 가정
  - d(s, t) : 정점 s와 정점 t의 거리
  - t : x, y를 연결하는 경로와 u, v를 연결하는 경로가 공유하는 한 점
- x에서 가장 먼 정점이 y이므로 **d(t, y) >= max(d(t, u), d(t, v))**
- 그러나, d(t, y) > max(d(t, u), d(t, v))인 경우는 u, v를 연결하는 경로가 지름이라는 가정에 위배됨
  - 왜냐하면 d(t, y) > max(d(t, u), d(t, v))가 참이면 아래 두 가지가 참이어야 하지만, 이 두 가지가 참이면 d(u, v)가 지름이라는 가정을 위배 
  1. **d(t, y) + d(t, v) > max(d(t, u) + d(t, v), d(t, v) + d(t, v))**
  2. **d(t, y) + d(t, u) > max(d(t, u) + d(t, u), d(t, v) + d(t, u))**
- **따라서, d(t, y) = max(d(t, u), d(t, v)) 이므로 트리의 지름을 구할 수 있다.**

<br>
<br>

### **case 3-2. x와 y를 연결하는 경로가 u와 v를 연결하는 경로와 독립**

<br>

- 이 경우, u에서 가장 먼 점이 정점 v가 아닌 x 또는 y가 됨
- 따라서, d(u, v)가 지름이라는 가정에 위배되어 case 3-2는 발생하지 않음

<br>

---


**참고 자료**


-   [트리의 지름 구하기](https://blog.myungwoo.kr/112)

<br>