---
title: "AI & Numpy"
tags: ["Analysis", "Numpy", "AI"]
---



# AI & Numpy

##### 2019. 6. 24 (Mon)

<br>

## Machine Learning(기계 학습)

<hr>

머신러닝: 인공 지능의 한 분야. 컴퓨터가 학습할 수 있도록 하는 알고리즘과 기술을 개발하는 분야


### 1. ML과 각 키워드 비교

#### 1. ML vs Big data?

ML: big data를 해석하는 방법중 하나

Big data: 그냥 데이터가 많음을 의미

<br>

#### 2. ML vs Data mining?

Data mining: 정형화된 데이터를 중심으로 분석하고 이해하고 예측하는 분야. 정형 데이터(테이블)를 분석하는데 사용

ML: 비정형 데이터를 분석하는데 사용

<br>

#### 3. ML vs AI?

AI: 인간의 지능을 컴퓨터가 갖게 하는 기술

ML: AI기술중 데이터에 의존하는 방법

<br>

#### 4. ML vs Statistic?

ML은 통계학이 만들어 놓은 분석 방법을 데이터에 적용하여 통계학 분석의 한계를 극복.

<br>

<br>

### 2. ML 기법 종류

#### 1. Supervised Learning(지도 학습)

Supervised learning: 정답을 주고 학습시키는 머신러닝의 방법론. 대표적으로 regression과 classification이 입니다.

지도한다: 정답이 정해져 있는 학습

학습 데이터로 학습. 테스트 데이터로 분류 및 판단

**학습 데이터 (Training data)**: 모델을 학습시킬 때 사용하는 데이터. 학습데이터로 학습 후 모델의 여러 파라미터들을 결정한다.

**테스트 데이터 (Test data):** 실제 학습된 모델을 평가하는데 사용되는 데이터

**선형 회귀 (Linear Regression):** 종속 변수 y와 한개 이상의 독립 변수 x와의 선형 상관 관계를 모델링하는 회귀분석 기법

<br>

#### 2. Unsupervised Learning

정답없는 데이터를 어떻게 구성되었는지를 알아내는 머신러닝의 학습 방법론

지도 학습 혹은 강화 학습과는 달리 입력값에 대한 목표치가 주어지지 않는다.

여러 종류 사진이 있을 때 비슷한 것을 찾는다?

데이터의 형태에 따라 몇 그룹으로 나누는지, 어떻게 나누는지를 결정

<br>

#### 3. Representation Learning(deep learning)

Neural Network: `reducing the dimensionality of data with neural network`

부분적인 특징을 찾는 것이 아닌 하나의 뉴럴 넷 모델로 전체의 특징을 학습하는 방식

현실에서 발생하는 문제를 종합적으로 해결하는 방식

<br>

<br>

<br>

## Numpy 관련 연산

<hr>

<br>

python에서 고성능 수치 계산 라이브러리

행렬 관련 연산을 쉽고 빠르게 도와준다

배열을 numpy 행렬로 변환시 ndarray객체로 변환

<br>

### 1. 배열 생성관련 함수

**np.array(list):** list를 넘파이 배열로 생성

**np.zeros(shape):** 0 이 들어있는 배열 생성

**np.ones(shape):** 1 이 들어있는 배열 생성

**np.empty(shape):** 초기화가 없는 값으로 배열을 반환

**np.arange(n ,m):** range 함수를 이용하여 배열을 생성

**np.ones(n).reshape(a, b)**: 1로된 n개의 배열을 생성한 후 a*b배열로 변환

**ndarray[n, m]:** n 행 m 열의 원소를 추출.

**ndarray[n, :]:** n 행을 추출.

**ndarray[:, m]:** m 열을 추출.

<br>

### 2. 배열의 통계적 정보를 나타내는 함수

**np.min(x):** 배열 x 의 최솟값

**np.max(x):** 배열 x 의 최댓값

**np.mean(x):** 배열 x 의 평균값

**np.median(x):** 배열 x 의 중앙값

**np.var(x):** 배열 x 의 분산

**np.std(x):** 배열 x 의 표준편차

<br>

#### 표준화

A = (A - np.mean(A))/np.std(A)

<br>

##### 사용 예시

```python
def matrix_nom_var():
    # [[5,2,3,0], [3,4,5,1], [3,2,7,9]] 값을 갖는 A 메트릭스를 선언합니다.
    A = np.array([[5,2,3,0], [3,4,5,1], [3,2,7,9]])

    # 주어진 A 메트릭스의 원소의 합이 1이 되도록 표준화 (Normalization) 합니다.
    A = (A - np.mean(A))/np.std(A)
    print(A)
    
    # 표준화 된 A 메트릭스의 분산을 구하여 리턴합니다.
    return np.var(A)
```

<br>

<br>

#### 3. 행렬의 연산과 관련된 함수

**np.transpose(x), (ndarray)x.T**: 배열 x의 전치 행렬을 나타낸다.

**np.dot(x, y)**: 배열 x와 y의 행렬곱을 나타낸다.

**(ndarray)x * (ndarray)y** : 행렬x와 y의 요소별 곱을 나타낸다.

**np.linalg.inv(x)**: 행렬 x의 역행렬을 배열로 나타낸다.

<br>

<br>

<br>

## Pandas

<hr>

데이터 분석 기능을 제공하는 라이브러리

csv, xls 등의 파일의 데이터를 원하는 형식의 데이터로 변환

정수형 인덱스에 명시적인 인덱스를 추가 가능

<br>

### 1. Series

`pd.Series`는 1차원 데이터를 다룰 때 사용하는 객체

변수를 출력하면, 인덱스 번호와 이름, 자료형도 함께 출력

<br>

**.Series(data, name)**: data를 name 이라는 이름의 `Series`형dmfh 변환

```python
def main():
    # Series()를 사용하여 1차원 데이터를 만들어보세요.
    # 5개의 age 데이터와 이름을 age로 선언해보세요.
    data = [19, 18, 27, 22, 33]
    age = pd.Series(data, ['a', 'b', 'c', 'd', 'e'])
    a = pd.Series([20, 15, 30, 25, 35], name='age')
```

```python
# dictionary 또한 변환 가능
class_name = {'국어' : 90,'영어' : 70,'수학' : 100,'과학' : 80}
    class_name = pd.Series(class_name)
```

<br>

<br>

### 2. DataFrame

`DataFrame`은 Series와 달리 여러개의 column을 가질 수 있다

`DataFram`e을 정의할 때는 2차원 리스트를 매개 변수로 전달하며, 여러개의 `Series` 데이터를 합쳐 `DataFrame`을 만들 수도 있다

<br>

**DataFrame(data)**: data를 `DataFrame` 구조로 변환

```python
data=[['name', 'age'],['철수',15],['영희',23],['민수',20],['다희', 18],['지수',20]]
    data = pd.Series(data)
```

<br>

<br>

### 3. Pandas 데이터 조작

#### 데이터 추출, 추가

**loc()**: 명시적인 인덱스를 참조하는 인덱싱/슬라이싱

**iloc()**: 정수 인덱스 인덱싱/슬라이싱. 마지막 인덱스는 제외

<br>

#### 데이터 삭제

**drop()**: DataFrame의 index, Column 삭제