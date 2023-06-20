---
title:  "Two Pointers"
excerpt: ""

categories:
  - Algorithm
tags:
  - [Algorithm, Two Pointers]

toc: true
toc_sticky: true
 
date: 2023-06-20
last_modified_at: 2023-06-20
---

<br>

# **투포인터**

<br>

## **개념**

<br>

- 각 원소마다 모든 값을 순회해야 할 때 : O(N^2)
- 연속하다는 특성을 이용해서 처리 : O(N)
- 두 개의 포인터(커서)가 움직이면서 계산

<br>
<br>

## **TIP**

<br>

- 처음부터 생각하기 어려움. 쉬운 방법부터 생각
    - O(N^2) 시간복잡도 초과한다면
    - 연속하다는 특징 사용할 수 있는지 확인
- 투포인터 문제 종류
    - 두 개 다 왼쪽에서 / 각각 왼쪽, 오른쪽 / 다른 배열
    - 시간복잡도 : 일반 O(N) / 정렬 후 투포인터 : O(NlogN)

<br>

## **[관련 문제 : 백준 2470 두 용액](https://www.acmicpc.net/problem/2470)**

<br>

**풀이**

<br>

```java
import sys
input = sys.stdin.readline

N = int(input().rstrip())
liquids = list(map(int, input().rstrip().split())) # 용액의 특성값 N개
liquids.sort() # 투포인터 이용하기 위해 정렬

# 포인터 두 개
start = 0
end = N - 1
ans = [liquids[start], liquids[end]]
sumv = abs(liquids[start] + liquids[end])

while True :

    tmp = liquids[start] + liquids[end]
    if sumv > abs(tmp) :
        sumv = abs(tmp)
        ans = [liquids[start], liquids[end]]

    if tmp < 0 : start += 1 // 합이 0보다 작으면 음수 쪽 포인터를 옮겨서 0과 가까워지도록
    elif tmp > 0 : end -= 1 // 합이 0보다 크면 양수 쪽 포인터를 옮겨서 0과 가까워지도록
    else : break

    if start >= end : break

print(*ans)
```

<br>

---

<br>

**참고 자료**

<br>

- [코딩테스트 알고리즘 - 5. 투포인터](https://youtu.be/U0TXIFiCIu0)

<br>