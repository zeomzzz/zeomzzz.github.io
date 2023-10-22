---
title:  "Knapsack"
excerpt: ""

categories:
  - Algorithm
tags:
  - [Algorithm]

toc: true
toc_sticky: true
 
date: 2023-10-22
last_modified_at: 2023-10-22
---

<br>

# **1. 0-1 Knapsack**

<br>

## **풀이 방법**

- 문제 상황 : 배낭의 최대 용량 W, 보석이 N개 있는데, 각 보석의 무게와 가격을 알고 있다. 배낭에 담을 수 있는 최대의 보석 가격의 합 구하시오.
- DP를 사용
    - 2차원 배열 사용 : r : 보석 번호 0 ~ N, c : 최대 용량 0 ~ W
    - i번째 보석을 담을 때, i번째 보석을 담지 않을 때 중 max를 선택
- Greedy로 풀이 불가능
- 코드
    
    ```python
    def knapsack(W, wt, val, n): # W: 배낭의 무게한도, wt: 각 보석의 무게, val: 각 보석의 가격, n: 보석의 수
        dp = [[0 for x in range(W+1)] for x in range(n+1)]
        for i in range(n+1): # 보석
            for w in range(W+1): # 무게 한도
    			# 0번째 행/열은 0
                if i == 0 or w == 0 : dp[i][w] = 0
    			# i번째가 무게 한도보다 적음
    			# i번째 넣는 경우 : i 가격 + (i - 1)까지 썼을 때 (무게 한도 - i의 무게)까지의 최대 가격
    			# i번째 안넣는 경우 : w 무게 제한 내에서 i-1까지 범위 내에서 이용해서 담는 경우
                elif wt[i] <= w : dp[i][w] = max(val[i]+dp[i-1][w-wt[i]], dp[i-1][w])
    			# i번째가 무게 한도보다 무거움 -> 담는 것 아예 불가능
                else : dp[i][w] = dp[i-1][w]
    
        return dp[n][W]
    ```

<br>

## **관련 문제**

- [BOJ 수강 과목](https://www.acmicpc.net/problem/17845)

<br>

---

**참고 자료**

<br>

- [Dynamic Programming: 배낭 채우기 문제 (Knapsack Problem)](https://gsmesie692.tistory.com/113)

<br>