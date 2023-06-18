---
title:  "Sliding Window"
excerpt: ""

categories:
  - Algorithm
tags:
  - [Algorithm, Sliding Window]

toc: true
toc_sticky: true
 
date: 2023-06-18
last_modified_at: 2023-06-18
---

<br>

# **슬라이딩 윈도우**

<br>

- N개의 원소를 갖는 배열, W의 넓이를 갖는 창문
- 창문을 왼쪽부터 시작하여 한 칸씩 오른쪽으로 이동
- 매 순간 창문 안에서의 정보 유출 필요
- 기본 아이디어 : Window를 한 칸 옮기면 (W - 1) 칸은 겹침
  -> Window를 한 칸 옮길 때 W개를 전부 다 더하는 작업을 하지 않고, 이전의 결과를 써먹음

<br>
<br>

## **시간 복잡도 : O(N)**

<br>

- 기존 : O(NW) : 모든 창문 위치마다 O(W)에 합을 구함
- 슬라이딩 윈도우는 한 칸씩 밀 때에 O(1)에 합을 구할 수 있으므로 O(N)

<br>

---

<br>

**참고 자료**

<br>

- [[알고리즘 강의] Sliding Window, 슬라이딩 윈도우](https://www.youtube.com/watch?v=uH9VJRIpIDY)

<br>