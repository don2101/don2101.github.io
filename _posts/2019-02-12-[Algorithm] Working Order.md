---
title: "Working Order"
tags: ["Topological Sort", "Algorithm"]
---



# Working Order

##### 2019. 02. 12 (Tue)

<br>

일의 순서와 소요 시간을를 그래프로 표현한 것

이때 일의 순서를 구하는 알고리즘

<br>

<br>

## 1. Topological Sort

1. 진입차수가 0인 것을 소거 후 출력
   1. 간선도 같이 제거
2. 노드 수가 0이 될 때 까지 1.을 반복

<br>

<br>

<br>

## 2. 역방향 DFS

1. 진입차수가 0인 노드를 시작노드로 설정
2. 모든 간선을 역방향처리
3. 시작노드 부터 dfs 수행
4. 더 갈곳이 없을 때 출력하고 return
   1. 갈곳이 있으면 출력하지 않고 dfs수행
5. 3.번부터 반복수행