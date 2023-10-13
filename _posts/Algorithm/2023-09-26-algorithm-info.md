---
title:  "알고리즘 풀이 참고사항"
excerpt: ""

categories:
  - Algorithm
tags:
  - [Algorithm]

toc: true
toc_sticky: true
 
date: 2023-09-26
last_modified_at: 2023-09-26
---

<br>

# **1. 풀이 아이디어**

- 거리에 관한 문제 → bfs, dfs

<br>

**# 2. 주의사항**

- pop할 때 : len == 0 확인
- 0인 케이스 고려했는지 주의

<br>

**# 3. 메소드**

**## 1) Python**

- heapq
    - import heapq
    - `heapq.nlargest(n, pq)` : pq에서 최대값 n개를 list로 반환
    - `heapq.nsmallest(n, pq)`
    - `heapq.heappush(pq, n)` : n을 pq에 push

- del vs. remove
    - `del` : index로 제거 ex. del list[0]
    - `remove` : 값으로 제거. 가장 첫 번째 것만 ex. list.remove(3)

- zip
    - 두 개의 리스트의 요소를 순서대로 튜플로 묶어줌
    
    ex. 
    
    ```python
    item = ['candy', 'coffee']
    price = [500, 1000]
    
    z = zip(item, price)
    print(list(z))
    
    >> [('candy', 500), ('coffee', 1000)]
    ```

<br>

**## 2) Java**

<br>