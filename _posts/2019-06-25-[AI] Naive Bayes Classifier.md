---
title: "Naive Bayes Classifier"
tags: ["Naive Bayes Classifier", "Scikit-learn"]
---



# Naive Bayes Classifier

##### 2019. 06. 24 (Mon)

<br>

확률 및 통계를 기반하는 베이즈 정리를 적용한 알고리즘

**Bayes rule**: 조건부 확률로 분류

**중요 assumption**: 모든 feature가 독립적이다

- 그러나 실제로 항상 그렇지는 않다

<br>

#### Naive Bayes를 사용하는 이유?

인간의 상식적인 선에서 분류하는 모델

실제 연구에서 가장 기본적인 모델로 사용

그러나 성능은 그렇게 좋지 않다.

<br>

#### Feature

사물을 분류하는데 사용하는 성질, 판단하는 기준

data에 대한 detail

분야마다 feature가 다르며 사용하는 분류 방법이 다르다

컴퓨터에게는 pixel로 되어있는 비정형 데이터를 숫자 데이터로 변환해야 한다.

데이터 분석을 위해 data에서 feature를 추출하고 수치화 해야 한다.

<br>

<br>

<br>

## Naive Bayes Model

<hr>

추출한 feature를 토대로 분류를 진행하는 모델

<br>

### 1. Naive Bayes Model

prior: 데이터를 보지 않은 상황에서 Y 클래스에 대한 확률

Prior가 uniform distribution이 아닐 경우도 있다.

$Fi,j$: feature 정보

<br>

##### Conditional Probability Table

다른 변수와 관련된 단일 변수의 조건부 확률을 표시하기 위한 변수들의 집합

각 feature에 대한 Y의 확률

![]()

<br>

결국 naive bayes를 학습한다는 것은 CPT를 알아내는것

답은 training data에서 empirically 학습하여 추출한다

<br>

#### text가 data 일 때

$P(Y) = P('free'|spam)$?

단어의 frequency와 어떤 문맥에 속한 단어인지가 중요

<br>

#### Bag-of-Words

단어가 몇 번 나오느냐만 따진다.

단순하기 때문에 사용

<br>

<br>

### 2. Smoothing

통계 및 이미지 처리에서 데이터 세트를 매끄럽게 해주는 방법

data가 abnormal하여 이상한 확률이 등장하여 overfitting 발생하는 것을 방지

한번도 안나온 확률에 대해 0이되는 것을 방지

$ number+k/(number~of~sample+k)$: 분모, 분자에 k를 추가하여 확률이 0이 되는 것을 방지한다.

어떻게 k를 찾아낼 것인가? 또한 중요한 문제

<br>

##### held-out data(validation)

k(hyperparameter)를 찾기위한 데이터

test데이터와 유사한 값이 나와야 한다.

보통 1000개 데이터

- 800 training
- 100 validation
- 100 test

<br>

<br>

### 3. Error가 발생하는 경우에는?

더 많은 feature가 필요

<br>

#### spam메일 분류가 안된다?

더 많은 feature words를 사용

- sender가 누구인가?
- spam 메일의 또 다른 특징 적용

<br>

<br>

### 4. Feature

Naive Bayes에서는 feature를 임의의 숫자로 한정한다

하지만 보통은 실제 값이 들어간 함수로 나타내며, 계산이 필요한 경우 또한 있다.

feature 파악을 위해 해당 도메인 지식 또한 필요

<br>

<br>

### 5. Scikit-learn

머신러닝을 위한 다양한 라이브러리를 내장한 모듈

<br>

##### scikit-learn을 활용한 뉴스 데이터 분류

```python
from sklearn.datasets import fetch_20newsgroups             # 20 News group 데이터 로드
from sklearn.feature_extraction.text import CountVectorizer # 단어를 Bag of Word로 만들기 위한 모듈
from sklearn.naive_bayes import MultinomialNB               # 다항분포 나이브 베이즈 모델
from sklearn.metrics import accuracy_score                  # 정확도 계산을 위한 모듈

# Train & Test 데이터 준비
newsdata=fetch_20newsgroups(subset='train')
newsdata_test = fetch_20newsgroups(subset='test', shuffle=True) 

# 데이터 분석
print('데이터 속성                  : ',newsdata.keys())
print('Train 데이터 개수            : ',len(newsdata.data))
print('Train 데이터의 Label 개수    : ',len(newsdata.target))
print('Train 데이터의 카테고리 개수 : ',newsdata.target_names,'\n')

# 뉴스 데이터의 단어를 학습 가능하도록 BoW로 변환
tdmvector = CountVectorizer()
X_train_tdm = tdmvector.fit_transform(newsdata.data)
print('Train Data의 개수, Data안의 단어 개수 : ',X_train_tdm.shape)

# 사이킷런에 내장되어 있는 Naive-Bayes 모델 불러오기
mod = MultinomialNB()

# 뉴스 데이터 학습
mod.fit(X_train_tdm, newsdata.target)

# Test 데이터를 BoW으로 변환
X_test_tdm = tdmvector.transform(newsdata_test.data) 

# Test 데이터에 대한 예측
predicted = mod.predict(X_test_tdm) 
print("Test 데이터 정확도 : ", accuracy_score(newsdata_test.target, predicted)) #예측값과 실제값 비교
```

<br>

##### Bayes Probability 예시

```python
def main():
    sensitivity = float(input())
    prior_prob = float(input())
    false_alarm = float(input())

    print(mammogram_test(sensitivity, prior_prob, false_alarm))

def mammogram_test(sensitivity, prior_prob, false_alarm):

    """
    x=1 : 검진 결과 유방암 판정
    x=0 : 검진 결과 유방암 미판정
    y=1 : 유방암 발병됨
    y=0 : 유방암 미발병
    """

    # The likelyhood probability : 유방암을 가지고 있는 사람이 검진 결과 유방암 판정 받을 확률
    p_x1_y1 = sensitivity

    # The prior probability : 유방암을 가지고 있을 확률로 매우 낮다.
    p_y1 = prior_prob

    # False alram : 유방암을 가지고 있지 않지만 검사 결과 유방암 판정을 받을 확률
    p_x1_y0 = false_alarm
    
    p_x1 = false_alarm*(1-prior_prob) + sensitivity*prior_prob
    
    # Bayes rule 
    p_y1_x1 = p_x1_y1*prior_prob/p_x1

    # 검사 결과 유방암 판정을 받은 환자가 정확한 검진을 받았단 확률
    return p_y1_x1

if __name__ == "__main__":
    main()

```





