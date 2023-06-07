---
title:  "Disjoint Set"
excerpt: "Disjoint Set 이론 정리"

categories:
  - Algorithm
tags:
  - [Algorithm, Disjoint-Set]

toc: true
toc_sticky: true
 
date: 2023-05-07
last_modified_at: 2023-05-07
---

<br>

# **서로소 집합 (Disjoint Set)**

<br>

-   서로소 집합 == 상호배타 집합

<br>
<br>

## **서로소 집합 연산**

<br>

| 연산 | 설명 |
| --- | --- |
| Make-Set(x) | x가 대표자인 집합 생성 |
| Find-Set(x) | x가 포함된 집합의 대표자를 반환 |
| Union(x, y) | x, y가 포함된 집합을 하나의 집합으로 만듦 |

<br>

-   아래와 같은 테이블을 이용하여 연산

| 노드 |   |   |   |   |
| --- | --- | --- | --- | --- |
| p (노드의 부모) |   |   |   |   |
| rank (노드의 트리 랭크 값) |   |   |   |   |

<br>

-   Make-Set(x)

```
def Make-Set(x) :
	p[x] = x
    rank[x] = 0
```

<br>

-   Find-Set(x)

```
def Find-Set(x) :
	if x != p[x] :  # x가 루트가 아닌 경우
    	p[x] = Find-Set(p[x])  # x의 부모에 대해서 Find-Set (파라미터가 루트일 때까지)
	return p[x]  # x가 루트이면 return
```

<br>

-   Union(x, y)

```
def Union(x, y) :
	p[Find-Set(y)] = Find-Set(x) # y의 대표자를 x의 대표자로 바꿈
```

<br>

cf) Rank 사용하는 경우

```
def Union(x, y):
    Link(Find_Set(x), Find_Set(y))

def Link(x, y):
# rank가 더 작은 것을 더 높은 곳에 붙임
    if rank[x] > rank[y]:
    	p[y] = x
    else:
    	p[x] = y
# rank가 같은 경우에는 둘 중 하나를 루트로 설정하고, 루트로 설정한 노드의 rank를 ++
    if rank[x] == rank[y]: 
    	rank[y] += 1
```

<br>

---

<br>

**참고 자료**

<br>

-   [Algorithm. 그래프 - 상호배타 집합](https://ohdowon064.tistory.com/208)
