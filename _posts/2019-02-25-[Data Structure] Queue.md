---
title: Queue
tags: ["Queue", "Priority Queue", "Data Structure"]
---



# Queue

##### 2019. 02. 25 (Mon)

<br>

자료를 밀어 넣어 저장하는 자료구조

한쪽에서 자료를 밀어 넣고 다른 한쪽에서는 자료를 빼서 참조한다.

**First In First Out**: 먼저 들어온 것이 먼저 나간다.

<br>

<br>

## I. 기본 구조 & 연산

front: Queue의 앞 위치

rear: Queue의 끝 위치

enqueue: rear에 데이터 삽입

dequeue: front의 데이터 제거

<br>

<br>

### 1. 연산과정

초기상태: front == rear == -1

큐가 비어있는 상태: front == rear

front: dequeue일 때 값이 증가

rear: enqueue일 때 값이 증가

포화상태: rear == n-1

<br>

<br>

### 2. 선형 큐의 문제점

잘못된 full 인식: 배열에 모든 자료가 다 차있지 않은데도 rear==n-1면 자료가 모두 차있는 것으로 생각할 수 있다.

**나머지 연산**으로 원형큐를 만들어 해결

<br>

<br>

### 3. 적용 예시

버퍼: std input/output queue

<br>

<br>

<br>

## II. 원형 큐

큐의 앞과 뒤를 이은 큐

전체 배열 크기 - 1만큼 요소 저장 가능

<br>

### 1. 연산과정

초기상태: front == rear == 0

배열의 크기만큼 mod연산자 사용

삽입 위치: rear = (rear + 1) % n

삭제 위치: front = (front + 1) % n

- front는 비어있어야 한다

포화상태: **삽입할 rear의 다음 위치 = 현재 front**

- (rear + 1) % n == front

<br>

<br>

<br>

## III. 연결 리스트 큐

노드를 이어서 큐를 구현

크기의 제한이 없다

<br>

### 1. 상태 표현

노드의 다음에 오는 노드의 주소를 저장

초기 상태: front == real = null

공백 상태: front == real = null

<br>

<br>

### 2. 연산 과정

#### enqueue

원소가 1개: front와 rear가 같은 원소를 가리킴

원소가 2개: front의 **다음 노드가 rear**

**노드를 연결**하는 과정 필요

삽입 후 rear는 새로운 노드를 가리킴

<br>

#### dequeue

삭제 후 front는 다음 노드를 가리킴

<br>

<br>

<Br>

## IV. 우선순위 큐

우선순위를 가진 항목을 저장하는 큐

FIFO 순서가 아니라 우선순위가 높은 순서대로 나가게 된다.

적용 분야: 네트워크 트래픽 제어, 운영체제 스케쥴링

<br>

### 1. 연산 과정

큐에 데이터가 들어왔을 때 우선순위에 따라 정렬

<br>

<br>

### 2. 구현 방법

#### 배열 이용

selection sort처럼 삽입시 우선순위에 따라 정렬하며 저장

#### 노드 구현

front에서 부터 검색해서 노드 삽입

<br>

<br>

<br>

## V. 기타 내용

멀티쓰레드
- 쓰레드: 한 프로그램의 **작업을 나눈 단위**
  - ex) 카톡: 알림 매니저, 화면 출력, 메세지 매니저, 메세지 입력...

<br>

<br>

### 운영체제 기본 I/O

- standard Input: (default)키보드 입력. 이후 파일 등 매체 입력으로 변환 가능

- standard Output: (default)스크린.

- standard Error: 에러 출력 device. (default)스크린.

