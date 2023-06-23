---
title:  "MySQL 문법 정리"
excerpt: ""

categories:
  - MySQL
tags:
  - [DB, MySQL]

toc: true
toc_sticky: true
 
date: 2023-06-23
last_modified_at: 2023-06-23
---

<br>

#### **MAX(IF)**

<br>

```
SELECT MAX(IF(조건, 1, 0))
```
- 모든 결과가 1이면 1, 0이면 0, 1, 0이 함께 존재하면 1 출력
- 관련 문제 : [프로그래머스 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기](https://school.programmers.co.kr/learn/courses/30/lessons/157340)

<br>