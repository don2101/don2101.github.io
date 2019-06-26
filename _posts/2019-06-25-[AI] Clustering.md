---
title: "Clustering"
tags: ["Clustering", "Big data"]
---



# Clustering

##### 2019. 06. 25 (Tue)



## Clustering 개념

<hr>

#### clustering?

데이터를 **유사도**에 따라 k개의 그룹으로 나누는 것

데이터들을 패턴에 따라 분류하고 그룹짓는 과정

<br>

#### 왜 clustering을 하는가?

어떠한 분야를 공부함에 있어 일단 대상을 분류하는것은 학습에 도움을 준다

머신러닝 분야에서 컴퓨터가 학습을 할 때 데이터가 분류되어 있으면 학습을 지도하는데 도움을 줄 수 있다.

또한, 추천시스템에도 사용 가능

<br>

#### clustering에 걸리는 시간

n개의 데이터를 k개의 group으로 분리한다면, k^n개만큼의 부분집합(clustering할 수 있는 경우의 수)이 존재한다.

따라서, 일일이 다 알아보는 것은 시간이 오래 걸리며, 좋은 분류 모델을 사용해야 한다.

<br>

<br>

<br>

## Clustering 모델

<hr>

### 1. k-means clustering

1. 일단 데이터를 k개를 그룹으로 랜덤으로 나눈다

2. 각 그룹의 center point를계산한다
3. 각 데이터와 가장 가까운 cluster center로 assign
4. 2.~ 3. 반복

<br>

#### 문제??

cluster의 크기가 다를 수 있다.

density가 낮으면 적용이 어렵다

구형일 때만 잘 적용된다

- 거리를 기반으로 하기 때문에

outliers problem

<br>

##### K-means clustering python 예시

```python
%matplotlib inline
import pandas as pd
import matplotlib.pyplot as pit
from sklearn.cluster import KMeans

df = pd.read_csv('cluster2.csv')
print("dimensions of the data = {}".format(df.shape))

x = df.values

# k-means 결과
k_means = KMeans(n_clusters = 4, random_state = 0, max_iter=100)

# 각 clustering 결과에 색을 적용
y_pred = k_means.fit_predict(x)

print(y_pred[500:600])

# cluster들의 center 출력
print(k_means.cluster_centers_)

# figure 객체 생성, size 설정
pit.figure(figsize=(5,5))

# c = 색 번호 입력
pit.scatter(x[:, 0], x[:, 1], c=y_pred, s=4, cmap=cmap)

#출력
pit.show()
```

<br>

<br>

### 2. Hierarchical Clustering

각 점들간 거리로 group을 정하는 모델

<br>

#### 점들간 거리를 구하는 방법

**single-link**

두 cluster 사이 거리는 모든 점 간 거리중 최소값

이 거리 보다 짧으면 하나의 cluster

1. 연결 안된 점을 탐색
2. 가장 최소거리가 작은 cluster를 group 으로 선택

<br>

**complete-link**

두 cluster 사이 거리는 모든 점 간 중 최대값

이 거리보다 짧으면 하나의 cluster이다

<br>

**average-link**: 각 데이터간 거리중 평균 구현

**central-link**: 중간값 비교

<br>

##### Hierarchical Clustering python 예시

```python
from sklearn.cluster import AgglomerativeClustering as ag

df = pd.read_csv('cluster2.csv')
x = df.values

# cluster를 변경시키며 출력
for j in range(2, 5):
    for i, linkage in enumerate(('single', 'complete', 'average', 'ward')):
    	# linkage: grouping 방법 설정
        clustering = ag(linkage=linkage, n_clusters=j)

        y_pred = clustering.fit_predict(x)

        # enumerate로 번호를 붙여서 여러 그래프를 한번에 입력
        pit.figure(i+1, figsize=(5,5))
        # 데이터 입력
        pit.scatter(x[:, 0], x[:, 1], c=y_pred, s=4, cmap=cmap)
        # title 입력
        pit.title(linkage+"# of cluster: {}".format(j))
    
    pit.show()

```

<br>

<br>

### 3. DBScan Clustering

density-base clustering 방법중 하나

density-connected points를 모아 group으로 구성

<br>

**장점**

- noise handling 가능
- 한번에 계산 가능
- density를 정해 분류

<br>

<br>

### 4. EM Clustering

가우시안 분포를 통해 group을 정하는 방법

<br>

##### python 구현 예시

```python
from sklearn.mixture import GaussianMixture

# n_components: the number of mixture components
em = GaussianMixture(n_components=4, max_iter=20, random_state=0)
y_pred = em.fit_predict(x)

print(y_pred)
print(em.weights_)
print(em.means_) 
print(em.covariances_)

pit.figure(figsize=(5, 5))
pit.scatter(x[:, 0], x[:, 1], cmap=cmap, s=8, c=y_pred)
pit.show()
```

<br>

<br>

### 5. clustering을 도와주는 도구

#### weka

data mining 분석을 위한 tool

기본적인 clustering 구현 및 데이터, 그래프를 확인할 수 있는 프로그래미

cvs, arff등의 확장자지원

<br>

#### arff 파일

weka에서 사용하는 파일 확장자

annotation으로 attribute, title등을 설정

csv파일과 annotation의 차이만 있다

<br>

<br>

### 6. PLSI

문서 자체에 여러 topic이 섞여 있다.

이러한 topic을 파악한다.











