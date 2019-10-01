---
title: "Indexed tree"
tags: ["Data structure", "Indexed Tree"]
---



## 1. Indexed Tree 기본 개념

#### 사용 이유

- 배열이나 리스트의 특정 구간에 대해 최대값, 최소값, 구간합 을 O(log(n))시간 안에 구하기 위한 자료구조



### 자료 저장 방식

- 이진 트리 구조에 자료를 저장한다

- Leaf Node와 Parent Node에 저장하는 값이 다르다

  

#### Leaf Node에는

- 기본적인 자료의 값이 들어간다



#### Parent Node에는

- 두 Children 노드의 값을 합한(혹은 최대값, 최소값)값이 들어간다
- Root Node에 도달할 때 까지 동일한 특성



### 배열에 저장할 때

- Root Node의 번호: 1
- 저장해야 하는 배열의 크기: n
- 배열의 n+1 부터 2n 까지의 배열에 자료를 저장
- Parent Node의 번호가 k이면 (2k, 2k+1)번 노드의 합, 최대값, 최소값을 저장한다



## 2. 메서드

### update()

- 자료의 값을 변경하는 메서드
- 특정 인덱스의 값을 변경한다



#### 작동 방식

1. 해당 인덱스의 값을 변경한다
2. 부모 노드로 올라가면서(/2를 하면서) 값의 차를 더해준다



### Sum(), Max(), Min()

- 해당 범위에 있는 구간합, 최대값, 최소값을 구한다



#### 작동 방식

- 구간의 시작: left, 구간의 끝: right

1. left의 번호가 홀수이면 해당 노드의 값을 연산(더하기, 최대 혹은 최소비교)하고 left + 1을 한다.
2. right의 번호가 짝수이면 해당 노드의 값을 연산하고 right-1을 한다.
3. left, right에 /2를 하여 부모 노드로 넘어간다

- left <= right 일 동안 1, 2, 3, 단계를 반복
- 반복이 끝난 뒤에 left == right 이면 해당 노드의 값을 연산하고 종료



> 예시 코드(구간합)

```c++
void update(int *tree, int position, int data) {
    int sumData = tree[position] - data;
    tree[position] = data;

    while(position > 1) {
        tree[position] += sumData;
        position /= 2;
    }
}


long long sum(int *tree, int left, int right) {
    long long sum = 0;

    while(left <= right) {
        if(left & 1) {
            sum += tree[left++];
        }
        if(right & 0) {
            sum+= tree[right--];
        }

        left /= 2;
        right /= 2;
    }

    if(left == right) sum += tree[left];

    return sum;
}
```

















