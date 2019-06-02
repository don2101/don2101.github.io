---
title: "Array"
tags: ["Data Structure", "Array"]
---



# Array

##### 2019. 01. 14 (Mon)

<br>

<br>

## I. 배열

자료형의 이름으로 열거하여 사용

같은 성질의 데이터를 묶어서 관리

<br>

### 1. 정렬 방식

#### 버블

O(n^2)

<br>

#### 카운팅

요소가 정수여야 함

배열을 잡을 수 있는 범위가 정해져 있어야함

쉬우면서도 강력한 알고리즘

정렬된 데이터를 복잡하게 사용하는 이유

- 출현한 순서대로 정렬

<br>

####선택

####퀵

####삽입

####병합

<br>

<br>

<br>

## II. Array 순회

### 1. 행 우선 순회

```python
for i in range(len(Array)) :
    for j in range(len(Array[i])) :
        Array[i][j]
```

<br>

<br>

### 2. delta를 이용한 2차원 배열 탐색

```python
ary[n][n]

dx[] = [0, 0, -1, 1]
dy[] = [-1, 1, 0, 0]

for x in range(len(ary)) :
    for y in range(len(ary[x])) :
        for i in range(4) :
            tx = x + dx[i]
            ty = y + dy[i]
            
            test(ary[tx][ty])
```

- 어느 좌표를 기준으로 상하좌우를 조사하여 결과를 낼 경우
- **이웃한 좌표**를 조사한다

<br>

<br>

### 3. 전치 행렬

```python
arr = #3*3 행렬

for i in range(3) :
    for j in range(3) :
        if i < j : # 한번 더 바꾸지 않는다
            arr[i][j], arr[j][i] = arr[i][j], arr[j][i]
```

<br>

<br>

### 4. Out of range 처리

Padding 처리 : 배열 밖을 영향 없는 숫자로 채워서 처리

isInside 처리 : 배열 밖을 계산에 넣지 않는다

<br>

<br>

<br>

## III. 부분집합 문제

#### {1, 2, 3}의 부분집합?

{}, {1}, {2}, {3}, {1, 2}, {2, 3}, {3, 1}, {1, 2, 3}

1, 2, 3중 **선택 하냐 마냐**의 경우

<br>

### 1. 구현 방법

loop 생성 : 경우의 수 만큼 0, 1을 선택하는 for문을 만들어 집합 생성

재귀 : 상황에 따라 함수를 실행하며 집합 생성

**완전 탐색 알고리즘!**

<br>

#### bit 연산

집합 관리

x = 1 : 000.....0001

수를 2진수 bit화 하여 **각 bit에 대해 연산**

&연산 : 1&1 = 1, 나머지는 0

|연산 : 0|0 = 0, 나머지는 1

^연산 : 서로 달라야 의미가 있다

<br>

#### shift 연산

x = x<<1 : 총 bit를 **왼쪽으로 1칸** 이동. 빈 bit = 0

**한번 <<** 마다 *2 연산

**한번 >>** 마다 /2 연산

> x = 1;
>
> x << n = x = 2^n;

##### n번째 bit가 0 or 1?

- i & (1<<j) > 0 : i의 j번째 bit가 0이냐 1이냐
  - 0이상 이면 1이고, 0 이하이면 0이다.

```
n = 3?
   000.... 0110
&) 000.... 1000 : 1 << 2
```

<br>

#### binary counting

```python
arr = [3, 6, 7, 1, 5, 4]
n = len(arr)

for i in range(1<<n) : # 2^n개 만큼의 부분집합 생성
    for j in range(n) : # 원소 수만큼의 bit를 비교
        if i & (1<<j) :
            print(arr[j], end=", ")
    print()
print()
```

- i = 0 : 공집합

<br>

<br>

<br>

## IV. 검색

탐색 키 : 자료를 구별하여 인식할 수 있는 키

<br>

### 1. 순차 검색

처음부터 마지막 까지 전부 조사

정렬한 상태라면 더욱 빠르다

O(n)

<br>

#### 보간 검색

정렬된 상태

등분을 기준으로 설명

<br>

<br>

### 2. 이진 검색

정렬이 된 상태에서 진행

**범위를 줄여가며** 반복실행

e = mid - 1, s = mid + 1

**분할 정복 알고리즘!**

```python
def binarySearch(a, key) :
    start = 0
    end = len(a)-1
    
    while start <= end :
        if a[mid] == key :
            return key	# success
        elif a[mid] > key :
            end = mid - 1
            mid = (start+end)//2
        else :
            start = mid + 1
            mid = (start+end)//2
    
    return False

#recursive
def binarySearch(a, high, low, key) :
    if low > high :
        return False
    mid = (high+low)//2
    
    if a[mid] == key :
        return false
    elif a[mid] > key :
        binarySearch(a, high-1, low, key)
    else :
        binarySearch(a, high, low+1, key)
```

- O(log(n))

<br>

<br>

### 3. 셀렉션 알고리즘

정렬되지 않은 배열에서 k번째 원소를 가져와라

정렬 이후 순서에 있는 원소 가져오기 : 는 오래걸린다

하나씩 최소값을 찾아 정렬하고, **base를 올려가며 찾아간다**

```python
def select(list, k) :
    for i in range(k) :
        minIndex = i
        for j in range(i, len(list)) :
        	if list[minIndex] > list[j] :
                minIndex = j
        temp = a[minIndex]
        a[minIndex] = a[i]
        a[i] = temp   
```









## 


