---
title: "Graph로 구현할 수 있는 것들"
tags: ["Graph", "Dfs", "Bfs", "Disjoint Set", "MST", "Dijkstra", "Floyd-Warshall"]
---



# Graph2

##### 2019. 04. 01 (Mon)

<br>

<br>

## I. 그래프 순회

### 1. Depth First Search(BFS)

**스택**을 사용하여 구현

<br>

#### 재귀적 방법

```PYTHON
def dfs_recursive(G, v):
    visited[v] = True
    visit(v)
    for w in range(len(G[v])):
        if not visited[w] and G[v][w]:
            dfs_recursive(G, w)
```

<br>

#### 반복적 방법

```python
def dfs_iterative(v):
    s = stack
    s.push(v)
    
    while not is_empty(s):
        v = s.pop()
        if not visited[v]:
            visited[v] = True
            visit(v)
            for w in range(len(G[v])):
                if not visited[w] and G[v][w]:
                    push(w)
```

<br>

<br>

### 2. Breadth First Search

**큐**를 사용하여 구현

큐에 삽입함과 동시에 방문처리

<br>

<br>

<br>

## II. Disjoint-Set

서로 **중복 포함된 원소가 없는 집합**들: 교집합이 없다

representative: 집합에 속한 **특정 멤버 하나**를 통해 각 **집합을 구분**

<br>

### 1. 연산

**Make-Set(x)**: x원소가 포함된 **집합 생성**

```python
def make_set(x):
    p[x] = x
```

<br>

**Find-Set(x)**: x가 **포함된 집합을 찾아 반환**

```python
def find_set(x):
    if p[x] == x: return x
    else: return find_set(p[x])
```

<br>

**Union(x, y)**: x가 속한 집합과 y가 속한 집합의 **합집합 생성**

```python
def union(x, y):
    p[find_set(y)] = find_set(x)
```

<br>

<br>

### 2. 표현 방법

#### 연결리스트

같은 집합의 represetative들을 하나의 연결리스트로 관리

연결리스트 맨 앞의 원소를 집합의 represetative 원소로 지정

각 원소는 represetative를 가리키는 링크를 갖는다

**Union**: 보통 **원소가 많은 쪽의 대표**를 합집합의 represetative로 설정

<br>

#### 트리

하나의 집합을 하나의 트리로 표현

자식 노드가 부모 노드를 가리키고, 루트 노드가 represetative가 된다

<br>

#### 문제점

부모까지의 높이만큼을 계속 탐색해야 함

중요한것: 저는 **무슨 집합입니다**

**어떤 집합에 속하는 지**만 저장하면 된다: **최종 부모의 값**만 저장

path_compression: **find 리턴시 부모를 최종 부모로 갱신**

```python
def find_set_path_compression(x):
    if p[x] == x: return x
    else: return p[x] = find_set(p[x])
```

<br>

<br>

<br>

## III. Minimum Spanning Tree

**모든 정점을 연결하는 간선들의 가중치의 합이 최소**가 되는 트리

두 정점 사이의 **최소 비용의 경로** 찾기

V-1개의 **edge**를 구한다: 가능한 edge중 V-1개의 edge를 뽑는다
- Spanning Tree: 원래 가능한 edge 집합 중 **V-1개의 edge를 뽑은** tree
- Minimum Spanning Tree: Spanning Tree들 중 **최소 비용**이 들어가는 tree

Kruskal 접근법: edge중심으로 해결

Prim 접근법: vertex중심으로 해결

<br>

### 1. 구현 방법

#### Prim Algorithm

1. 임의의 정점을 하나 선택해서 시작
2. 선택한 정점과 인접하는 정점들 중 **최소 비용의 간선의 정점을 선택**
3. 1., 2.를 반복

2개의 disjoint-sets 정보를 유지

- 트리정점: MST에 선택된 정점들
- 비트리 정점: 선택되지 않은 정점들

<br>

#### Kruskal Algorithm

1. 모든 간선을 오름차순으로 **정렬**
2. 가중치가 낮은 간선부터 선택
   1. **사이클이 존재하지 않도록** 다음 간선 선택
3. n-1개 간선이 선택될 때 가지 반복

- 문제점: 쉽지만, 정렬하는데 시간이 걸린다. 간선이 적은 경우에 선택

<br>

<br>

<br>

## IV. Shortest Path

노드간 최소 거리를 구하는 알고리즘

<br>

#### 경우에 따라 적용하는 알고리즘이 다르다

가중치가 **같다**, 모든 정점 경유 **X**: BFS

가중치가 **다르다**, 모든 정점 경유 **X**: 다익스트라(플로이드)

가중치가 **다르다**, 모든 정점 경유 **O**: TSP(순열, 가지치기, DP)

<br>

<br>

### 1. Dijkstra Algorithm

비 사이클, 양의 가중치에 대한 최소거리 알고리즘

시작 정점에서 거리가 **최소인 정점을 선택해 나가면서** 최단 경로를 구한다

**시작정점s**에서 **끝정점t** 까지 최단 경로에 **정점x**가 존재.

- 이때, 최단경로를 **s~x의 최단경로**와 **x~t까지 최단경로의 합**이다

<br>

<br>

### 2. Floyd-Warshall Algorithm

모든 쌍 최단경로

**각 점을 시작점**으로 정하여 다익스트라 알고리즘 적용

<br>

1. i, 1, j 가 있고, i~j최단 경로를 구한다
2. 1~k까지 사이 정점에 대해 최단거리를 구한다
3. i=1~n(i != k), j=1~n(j != k, j != i)까지 정점에 대해 수행

```python
def all_shortest_path(D):
    for k in range(n):
        for i in range(n):
            if i != k:
                for j in range(n):
                    if j != k and j != i:
                        D[i][j] = min(D[i][k]+D[k][j], D[i][j])
```

시작 정점에서 끝정점 사이의 모든 정점에 대한 값이 **누적**된다
- 1 ~ **2 ~ 5**
- **2 ~ 5**: min(2 ~ 1 ~ 5, 2 ~ 5)




