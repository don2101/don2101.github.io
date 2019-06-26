# Linear Regression

##### 2019. 06. 24 (Mon)



## Regression problem

종속 변수 yrhk 한 개 이상의 독립 변수 x와의 **선형 상관 관계**를 모델링하는 회귀분석 기법

주어진 x값에 대해 y값을 예측한다.

line-fitting을 수행하여 값을 찾아내는 작업



#### Line-fitting

데이터 set이 그래프 상에서 표현할 수 있을때, 여러 라인을 그릴 수 있다.

그중, 데이터 set에 가장 적합한 라인(y = ax + b)을 수학적으로 구하는 과정
$$
xy(x) = w^t*x + e
$$
의 형태에서 w와 e의 값을 찾는다.



#### non-linear-fitting

x대신 non-linear function 추가
$$
y = w^t*pi(x) + e~~(pi(x) :non-linear funtion,~w^t: coeffcient~array)
$$
**polynomial regression**: degree(차수)가 높은 regression 모델. 차수를 높이면 적합한 line을 찾을 수 있지만, 복잡도가 너무 높으면 overfit 될 수 있다.



#### Multivariate linear regression

2개 이상의 x변수에 대한 line-fitting

3차 이상의 차원에서 line-fitting



### 적합한 w?

#### Residual Sum of Squares

$$
sum(y_i - w^t*x_i)^2~~(i = 1~to~n)
$$



fitting한 line과 실제 데이터들의 차의 크기

실제 데이터와 예측값 사이의 오차를 의미하며, 이 차이를 줄이는 weight를 찾는다.



##### linear-fitting & RSS 예시

```python
import elice_utils
import matplotlib as mpl
mpl.use("Agg")
import matplotlib.pyplot as plt
import numpy as np
eu = elice_utils.EliceUtils()

def loss(x, y, beta_0, beta_1):
    N = len(x)
    
    '''
    x, y, beta_0, beta_1 을 이용해 loss값을 계산한 뒤 리턴합니다.
    '''
    
    length = len(x)
    err = 0
    for i in range(length):
        real_data = y[i]
        pred_data = beta_0*x[i]+beta_1
        err += (real_data-pred_data)**2
    
    return err

X = [8.70153760, 3.90825773, 1.89362433, 3.28730045, 7.39333004, 2.98984649, 2.25757240, 9.84450732, 9.94589513, 5.48321616]
Y = [5.64413093, 3.75876583, 3.87233310, 4.40990425, 6.43845020, 4.02827829, 2.26105955, 7.15768995, 6.29097441, 5.19692852]

beta_0 = 0.5 # 기울기
beta_1 = 2 # 절편

print("Loss: %f" % loss(X, Y, beta_0, beta_1))

plt.scatter(X, Y) # (x, y) 점을 그립니다.
plt.plot([0, 10], [beta_1, 10 * beta_0 + beta_1], c='r') # y = beta_0 * x + beta_1 에 해당하는 선을 그립니다.

plt.xlim(0, 10) # 그래프의 X축을 설정합니다.
plt.ylim(0, 10) # 그래프의 Y축을 설정합니다.
plt.savefig("test.png") # 저장 후 엘리스에 이미지를 표시합니다.
eu.send_image("test.png")
```



#### Sciket-learn

numpy내 기능으로, 자동으로 가장 적합한 line을 찾아주는 기능



##### Scikit-learn을 이용한 linear regression

```python
'''
여기에서 모델을 트레이닝.
'''
lrmodel = LinearRegression()
lrmodel.fit(train_X, train_Y)

# line의 기울기, 절편
beta_0 = lrmodel.coef_[0]
beta_1 = lrmodel.intercept_

print("beta_0: %f" % beta_0)
print("beta_1: %f" % beta_1)
```



#### Ridge Regression

Linear Regression에 L2 regularization을 사용하는 방법으로, 모델의 복잡도를 줄여서 좀 더 간단하고 부드러운 모델로 만들때 사용

linear-fitting을 할 때 over-fitting하지 않도록 주의한다. 

즉, 너무 complex한 모델(over-dimensioned curve, over-estimated weight)을 사용하면 오히려, fitting이 잘 안되는 경우가 있다.



#### (l2)Regularization

**정규화 (Regularization)**: weight가 너무 큰 값들을 갖지 않도록 하여 모델의 복잡도를 낮추는 방법

polynomial non-linear-fitting에 대해 미리 찾아놓은 **좋은 w값**을 제공

근데 미리 찾아놓은 값이 너무 크기 때문에 fitting을 복잡하게 한다.

그러니까 큰 w값에 penalty를 부여하여 over-fitting을 방지한다.



##### error를 사용하여 실제 선형 회귀 구현

```python
import numpy as np
import elice_utils
import matplotlib.pyplot as plt
import matplotlib as mpl
mpl.use("Agg")
eu = elice_utils.EliceUtils()
#학습률(gradient step)을 설정한다.(권장: 0.0001~0.01)
learning_rate = 0.0005
#반복 횟수(iteration)를 설정한다.(자연수)
iteration = 5000
def prediction(a,b,x):
    # 넘파이 배열 a,b,x를 받아서 'x*(transposed)a + b'를 계산하는 식을 만든다.
    equation = x*a + b
    
    return equation
    
def update_ab(a,b,x,error,lr):
    # a, b에 대해 error를 미분한 식(gradient)
    # a를 업데이트하는 규칙을 정의한다.
    delta_a = -(lr*(2/len(error))*(np.dot(x.T, error)))
    # b를 업데이트하는 규칙을 정의한다.
    delta_b = -(lr*(2/len(error))*np.sum(error))
    
    return delta_a, delta_b
    
    
def gradient_descent(x, y, iters):
    #초기값 a= 0, a=0
    a = np.zeros((1,1))
    b = np.zeros((1,1))
    
    for i in range(iters):
        #실제 값 y와 예측 값의 차이를 계산하여 error를 정의한다.
        error = -(prediction(a, b, x) - y)
        a_delta, b_delta = update_ab(a,b,x,error,lr=learning_rate)
        a -= a_delta
        b -= b_delta
        
    return a, b

def plotting_graph(x,y,a,b):
    y_pred=a[0,0]*x+b
    plt.scatter(x, y)
    plt.plot(x, y_pred)
    plt.savefig("test.png")
    eu.send_image("test.png")

def main():

    x = 5*np.random.rand(100,1)
    y = 3*x + 5*np.random.rand(100,1)
    
    a, b = gradient_descent(x,y,iters=iteration)
    
    print("a:",a, "b:",b)
    plotting_graph(x,y,a,b)
    
main()
```











### 선형 회귀 구현 예시

```python

```

