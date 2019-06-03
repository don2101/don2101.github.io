---
title: "Divide and Conquer"
tags: ["Algorithm", "Divide and Conquer", "Merge Sort", "Quick Sort", "Binary Search"]
---



# Divide and Conquer

##### 2019. 02. 18 (Mon)

<br>

<br>

## I. 분할 정복

문제를 크기 n/2인 부분문제로 나눈다.

나눈 문제를 해결하고 종합하여 최종 문제를 해결한다.

**Do Not Recompute!**

<br>

<Br>

### 1. 설계 전략

분할: 해결할 문제를 여러 개의 작은 부분으로 나눈다

정복: 나눈 작은 문제를 각각 해결한다

통합: 필요하다면 해결된 답을 모은다

<br>

##### 예시

3^10

직접하면 O(n)

3\*3\*3\*3\*3\*3\*3\*3\*3\*3

3\*3\*3\*3\*3 = 3\*3\*3\*3\*3

...

분할정복을 사용하면 O(log(n))

<br>

<br>

<br>

## II. 적용

### 1. 병합 정렬

Top - Down 방식

**분할 단계**: 최소 크기 부분집합이 될 때 까지 분할

**병합 단계**: 2개의 부분집합을 정렬하면서 하나의 집합으로 병합

- **2포인터** 사용하여 분할된 집합을 정렬하면서 하나로 병합

<br>

<br>

### 2. 퀵 정렬

#### 정렬 방법

pivot을 기준으로 작은 것은 왼쪽, 큰 것은 오른쪽으로 분할

pivot에 있는 수 기준으로 **왼쪽은 pivot보다 모두 작아야 한다**.

pivot에 있는 수 기준으로 **오른쪽은 pivot보다 모두 크거나 같아야 한다**.

<br>

L: 왼쪽부터 index 검사, R: 오른쪽 부터 index 검사

L과 R이 만나지 않았다면 L과 R을 교체

- L과 R이 만나고, L == pivot이면, pivot = R
- **L < R** 일동안 반복

R과 pivot 교체

결국 pivot은 **양 옆을 가르는 기준 숫자**

병합과정이 없다

<br>

#### Hoare partition 방법

1. 임시 pivot(첫번째 or 중간 요소) 설정
2. A[i] <= p 일 동안 i 오른쪽 이동
3. A[j] >= p 일 동안 j 왼쪽 이동
4. i < j면 A[i]와 A[j]를 swap
5. i <= j 일 동안 반복
6. pivot을 j 위치에 놓고 j를 return
7. 그러면 pivot 기준으로 왼쪽은 작은 것, 오른쪽은 큰 것만 존재

<br>

#### Lomuto partition 방법

1. pivot을 마지막 요소(r)로 설정
2. j = p, i = j-1에서 시작
3. A[j]가 pivot보다 작으면 i 1증가, A[i]와 A[j]를 swap
4. 그렇지 않다면 j만 증가
5. j가 r-1까지 실행
6. 그러면 pivot보다 큰 구간이 가장 오른쪽으로 옮겨짐
7. A[i+1]과 A[r]을 swap

<br>

##### 코드 예시

```python
def quickSort(a, begin, end) :
    if begin < end :
        pivot = partition(a, begin, end)
        quickSort(a, begin, p-1)
        quickSort(a, p+1, end)
        
def partition(a, begin, end) :
    # 임시 pivot
    pivot = (begin + end) // 2
    
    L = begin
    R = end
    
    while L < R :
        while(a[L] < a[pivot] and L < R) : L += 1
        while(a[R] >= a[pivot] and L < R) : R -= 1
        if L < R :
            # pivot과 R의 위치를 교환하지 않으면 pivot부터 R까지는 정렬이 되지 않는다.
            if L == pivot : pivot = R
            # L, R 위치 교체
        	a[L], a[R] = a[R], a[L]
    # R, pivot 위치 교체
    a[pivot], a[R] = a[R], a[pivot]
    
    return R
```

<br>

<br>

### 3. 이진 검색

정렬된 자료를 기준

중앙에 있는 원소를 비교하고, 작으면 왼쪽에 대해, 크면 오른쪽에 대해 새로 검색

low, high이용해 검색범위 설정

<br>

<br>

### 4. k - quick - selection

quick sort한번 실행

k번째가 포함된 범위만 다시 quick sort

<br>