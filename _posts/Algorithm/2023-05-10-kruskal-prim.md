---
title:  "MST - Kruskal, Prim"
excerpt: "Kruskal, Prim 이론 정리"

categories:
  - Algorithm
tags:
  - [Algorithm, MST, Kruskal, Prim]

toc: true
toc_sticky: true
 
date: 2023-05-10
last_modified_at: 2023-05-10
---

<br>

# **MST (Minimum Spanning Tree)**

<br>

## **MST**

<br>

-   Spanning Tree : 모든 노드가 연결된 트리
-   MST : 각 간선에 가중치가 있다고 할 때, 최소의 비용으로 모든 노드가 연결된 트리
-   알고리즘 : Kruskal or Prim
    -   그래프 내의 간선 숫자가 적은 sparse graph에서는 Kruskal, 간선 숫자가 많은 dense graph에서는 Prim이 적합

<br>
<br>

## **Kruskal**

<br>

-   전체 간선 중 가장 가중치가 작은 것부터 연결
-   Union-Find 이용
-   시간 복잡도 : `O(e log2e)`
-   코드

```
# 특정 원소가 속한 집합을 찾기
def find-set(x) :
	if x != p[x] :
    	p[x] = find-set(p[x])
    return p[x]

# 두 집합을 합치기
def union(x, y) :
	p[find-set(y)] = find-set(x)

# 간선 정보 입력 받기
# 비용순 정렬을 위해 w가 튜플의 첫 번째 원소가 되도록
edges = []
edges.sort()

# 초기화 : 부모가 자기 자신
parent = [0] * (v + 1)
for i in range(1, v + 1) :
	parent[i] = i

# 가중치 적은 간선부터 확인해서, 사이클 발생하지 않는 경우에 union 및 선택
for edge in edges :
	cost, a, b = edge
    if find-set(x) != find-set(y) :
    	union(x, y)
        result += w

print(result)
```
<br>
<br>

<br>
<br>

## **Prim**

<br>

-   현재 노드에 연결된 간선 중 가장 가중치가 작은 것을 추가
-   heap 이용 (최소 heap) : 노드에 연결된 간선 중 가중치가 최소인 간선이 가장 위로 올라옴
-   시간 복잡도 : `O(n^2)`
-   코드

```
# 1. 간선을 인접 리스트에 넣기
# edge = [[현재 idx 노드의 w, 연결된 node] ...]

# 2. 힙에 시작점 넣고 시작점은 방문처리
heap = [[0, 1]] # [가중치, 노드 번호]를 heap에 넣어줌
visited[1] = True

# 힙이 빌 때까지 반복
while heap :
	w, cur = heap pop # 남은 간선 중 가장 가중치가 적은 간선을 꺼냄
    
    if not visited[cur] : # 아직 방문 안한 노드면
    	visited[cur] = True # 방문 처리하고
        result += w # 간선 선택
        
        for next in edge[cur] : # 현재 노드랑 연결 된 노드 중
        	if not visited[next] : # 아직 방문 안했으면
            	heap push(heap, next) # heap push
```

<br>

---

<br>

**참고 자료**

<br>

- [19강 - 크루스칼 알고리즘(Kruskal Algorithm) \[ 실전 알고리즘 강좌(Algorithm Programming Tutorial) #19 \]](https://www.youtube.com/watch?v=LQ3JHknGy8c)
-   [코딩테스트 알고리즘 - 9. MST](https://youtu.be/nZ4RTuoHS_Y)
-   [Kruskal, Prim 알고리즘](https://keepdev.tistory.com/82)