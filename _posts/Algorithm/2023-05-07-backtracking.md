---
title:  "Backtracking"
excerpt: "Backtracking 이론 정리"

categories:
  - Algorithm
tags:
  - [Algorithm, Backtracking]

toc: true
toc_sticky: true
 
date: 2023-05-07
last_modified_at: 2023-05-07
---

<br>

# **백트래킹**

<br>
<br>

## **시간 복잡도**

<br>

- 중복 가능 : O(N ^ N)
- 중복 불가능 : O(N!)

<br>
<br>

## **코드 구조**

<br>

```
def backtracking(result, depth) :
	# base
    if :
    	return
    
    # recursive
    for i in range(N) :
    	backtracking(result, depth + 1)
        # 원복 코드
```