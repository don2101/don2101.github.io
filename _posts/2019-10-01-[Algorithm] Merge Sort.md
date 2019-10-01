---
title: "Merge Sort"
tags: ["Algorithm", "Merge Sort"]
---



## Merge Sort

<hr>

### 1. 기본 개념

- **분할 정복 문제**: 문제를 작은 단위로 분할하여 작은 문제부터 풀어 올라가는 방식
- 배열을 반씩 나눠가며 더이상 나눌 수 없는 부분까지 나눈다
- 더이상 나눌 수 없는 부분 부터 정렬해가며 배열을 완성

<br>

#### 수행시간

- O(n*log(n))

<br>

<br>

### 2. 작동 방식

#### 분할(divide)

- 배열을 반씩 나눈다
- 더 이상 나눌 수 없을 때 까지(원소가 1개) 나눈다

<br>

### 정복(merge)

- 분할된 두 배열을 비교한다
- 두 배열의 크기를 합만 만큼의 새로운 배열을 준비
- 두 배열의 원소를 처음부터 비교하며, 더 작은(혹은 큰) 원소를 새로운 배열에 할당
- 새로운 배열을 반환

<br>

<br>

> 예시 코드(c++)

```c++
// merge sort 실행
void mergeSort(int *array, int length) {
    divide(array, 0, length-1);
}

// 배열을 나누는 부분
void divide(int *array, int start, int end) {
    // case) start보다 end가 같거나 작으면 나눌 필요가 없다
    if(end <= start) return;

    // 1. 일단 두 부분으로 계속 나눈다.
    int mid = (start + end) / 2;
		
  	// 2. 재귀적으로 계속 나눠간다.
    divide(array, start, mid);
    divide(array, mid+1, end);
  
  	// 3. 나눈 두 배열을 병합
    merge(array, start, mid, end);
}

// 병합 부분
void merge(int *array, int start, int mid, int end) {
    // 1. i = 왼쪽 부분 시작, j = 오른쪽 부분 시작
    int i = start;
    int j = mid+1;
    int length = end-start+1;
    int *tempArray = new int[length];

    // 2. 정렬 부분, i에 있는 수와 j에 있는 수를 비교하며 tempArray에 저장
    for(int k = 0; k < length; ++k) {
        if (i > mid) tempArray[k] = array[j++];
        else if (j > end) tempArray[k] = array[i++];
        else if (array[i] <= array[j]) tempArray[k] = array[i++];
        else tempArray[k] = array[j++];
    }

    // 3. tempArray에 저장된 값을 다시 array로 옮김
    for(int p = start; p <= end; ++p) {
        array[p] = tempArray[p-start];
    }
}
```

<br>

### 구현시 주의해야 할 점

#### 1. 배열 포인트 제어

배열 부분에서 1차이 오류로 배열을 제대로 제어하지 못함

**해결방법**: 배열의 상황을 생각할 때 서로 침범하지 않는 범위로 나누는 것을 생각

<br>

#### 2. 정렬 제어

merge 할 때, 한쪽 배열의 정렬이 끝난 경우를 먼저 고려하지 않음

**해결법**: 우선되어야 하는 경우를 먼저 적용

<br>

<br>

### 3. 실행 속도 비교

#### Insertion sort와 비교

> 비교 test 코드

```c++
#include <ctime>
#include <cstdlib>
#include <time.h>

#define LARGE_NUM 1000000

const int arraySize = 10000;

void printArray(int *array, int length) {
    for(int i = 0; i < length; ++i) cout << array[i] << " ";
    cout << endl;
}

int getRandomSortNumber() {
    int num = rand() % LARGE_NUM + 1;

    return num;
}

void mergeSortTest() {
    srand((unsigned int)time(NULL));

    int array[arraySize];
    int start, end;

    for(int i = 0; i < arraySize; ++i) {
        array[i] = getRandomSortNumber();
    }

    start = clock();
    mergeSort(array, arraySize);
    end = clock();

    cout << "Merge Sort: " << (double)(end - start) << "ms" << endl;

    for(int i = 0; i < arraySize; ++i) {
        array[i] = getRandomSortNumber();
    }

    start = clock();
    insertionSort(array, arraySize);
    end = clock();

    cout << "Insertion Sort: " << (double)(end - start) << "ms" << endl;
}
```

<br>

#### 10000개 원소에 대해 실험한 결과

```
Merge Sort: 2166ms
Insertion Sort: 153854ms

Process finished with exit code 0
```

