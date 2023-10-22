---
title:  "Binary Search"
excerpt: ""

categories:
  - Algorithm
tags:
  - [Algorithm]

toc: true
toc_sticky: true
 
date: 2023-06-22
last_modified_at: 2023-10-22
---

<br>

## **개념**

- 어떤 값을 탐색할 때 정렬의 특징을 이용해 빨리 찾음
    - 항상 정렬이 되어 있어야 한다 !!!!
- 정렬되어 있을 경우 어떤 값 찾을 때 : O(N) -> O(logN)
    - 정렬이 안되어있는 경우
        - N개의 수 정렬 : O(N * logN)
        - M개의 수 이진탐색 : O(M * logN)
            
            -> O((N + M) log N)
            
- 처음부터 생각하기는 어려우므로 쉬운 방법부터 생각하자(시간 복잡도 따져보고 이진탐색으로)
- 입력개수가 10^5 인 경우 의심

<br>

## **핵심 코드**

- 무조건 외워두기
    
    ```java
    def searh(start, end, target) :
    	if start == end :
    		// ~~
    		return
    
    	mid = (start + end) // 2
    	if nums[mid] < target:
    		search(mid + 1, end, target)
    	else :
    		search(start, mid, target)
    ```
    
- 코드 설명
    - start : 시작하는 값
        
        end : 끝나는 값
        
        target : 찾는 값

<br>