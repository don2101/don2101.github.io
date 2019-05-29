# Python 제어 흐름

python의 조건에 따른 제어 흐름

<br>

<br>

## I. 조건문

#### 선언 방법

조건문에 반드시 ':' 을 사용해야함

**4칸의 indentation**을 사용할 것

- python은 **코드 블록을 indentation**으로 판단
- indentation이 정확하게 수행돼야 정상적으로 수행된다.

pass : 아무것도 하지 않고 지나간다

- 조건문 안에 어떤 명령문도 없다면 error 발생

<br>

#### 값을 비교할때?

and : 왼쪽 부분을 먼저 연산하기 때문에 **&** 보다 더욱 효율적
- &는 **양쪽 모두 연산**하기 때문에 연산 시간이 더욱 오래걸린다
- or도 **|**보다 효율적

**주의할 점** : 조건문 작성시 더 큰 범위에 해당하는 조건식을 뒤에 작성해야 

<br>

#### 복수 조건문

- elif \<조건식> : 2개 이상의 조건문 사용

<br>

#### 조건 표현식

**true_value** if \<조건식> else **flase_value**

- 더욱 짧게 코드 작성이 가능
- 조건에 따라 값을 정할 때 많이 활용

삼항 연산자 처럼 생각할 수 있다

구분이 실행된 뒤 조건식에 따른 value를 **반환한다**

<br>

#### list 안에 조건을 걸어 반환??

리스트에서 조건을 만족하는는 리스트를 다시 만들 때 다음과 같은 **조건 리스트** 를 사용한다.

```python
# score내의 원소중 average를 넘는 원소만 list로 반환
cnt = [s for s in score if s > average]
```

<br>

<br>

<br>

## II. 반복문

#### while 문

조건식이 **참일 동안** 반복적으로 코드 실행

- 조건식이 참인지 **매번 확인**

**4칸 indentation** 필수

<br>

#### for 문

정해진 범위 내(**시퀀스-순서**)에서 순차적으로 코드 실행

시퀀스 **첫번째 값 부터 마지막 값 까지** 수행

```python
for i in list1:
```

c나 java처럼 i를 하나씩 증가시키는게 아니라, 시퀀스 혹은 **range 내에 있는 값들을 하나씩 가져와서 i에 대입**

<br>

#### dictionary 반복문

- for문 iterate를 사용하여 가능
- dictionary에서 for문 사용하는 4가지 방법
- .items() : 모든 key, value 반환

```python
# 0. dictionary (key 반복)
for key in dict:
    print(key)

# 1. key 반복
for key in dict.keys():
    print(key)

# 2. value 반복    
for val in dict.values():
    print(val)

# 3. key와 value 반복
for key, val in dict.items():
    print(key, val)
```

<br>

<br>

#### else

반복문을 끝까지 시행한 이후에 실행

break를 통해 중간에 종료되지 않은 경우만 실행

iterator의 값은 마지막 값

```python
for i in range(10):
  if is_it_true:
    break
else:
  print(else)
```



<br>

<br>

### enumerate

sequence 자료형을 받아서 index와 value를 반환하는 **내장 함수**

sequence 자료형의 **value에 index를** 붙인다

index와 value를 **pair로** 인식

enumerate(\<list>, start = n) : n부터 index시작하는 enum 반환

>  몇개 제외한 list 반환 예시

```python
colors = ['Apple', 'Banana', 'Coconut', 'Deli', 'Ele', 'Grape']

fruit = [color for (a, color) in enumerate(colors) if a not in (0, 4, 5)]
print(fruit)

# result = ['Banana', 'Coconut', 'Deli']
```

<br>

<br>

<br>

## II. Comprehension

python스럽게 if문과 for문 코드를 짧게 축약시켜 작성하는 방법

<br>

### list comprehension

list에 조건을 추가하여 반환한다.

<br>

##### 기본 리스트

```python
[x for x in range(1, 5)]
```

<br>

##### 2배

```python
[x*2 for x in range(1, 5)]
```

<br>

##### dictionary

```python
{x:x*2 for x in range(1, 5)}
```

<br>

##### 중첩 반복

```python
[(girl, boy) for boy in boys for girl in girls]
```

<br>

##### 조건 추가

```python
[word for word in words if word not in ('a', 'e', 'i', 'o', 'u')]
```

<br>

<br>

### dicionary Comprehesion

key: value 형태로 표현

```python
cubic = {x: x**3 for x in range(1, 8)}
```
