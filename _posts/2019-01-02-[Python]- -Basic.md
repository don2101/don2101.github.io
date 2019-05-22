# Python basic

python 기초 개념 및 문법

 

 

## I.  Python?

- 다른 언어보다 간결하다.
- 생산성이 높다.
- 다양한 라이브러리

 

### (1) 저장

#### 1. 개념

파이썬은 **dynamic typing language** -> 자료형을 정의할 필요가 없다!

1. 숫자
2. 글자
3. Boolean

 

- 파이썬은 {}를 쓰지 않고 `:` 와 indentation을 사용
  - indentation은 4 space 기준(1 tab)



##### 변수 활용하기

```python
greeting = "안녕하십니까!!"	#변수에 데이터가 저장, 변경이 편하다

print("greeting")
print("greeting")
print("greeting")
print("greeting")
print("greeting")
```

변수를 저장하면 변경이 편하다



##### 리스트 저장

```python
menu = ["짜장면", "카레", "돈가스"]
```

박스를 여러개 만들어 이어 붙인 자료형



##### 딕셔너리 저장

```python
dust = {"강남구" : 10, "서초구" : 20}
```

자료 박스에 이름을 붙인 자료형



##### 반복문 for in(iteration)

```python
dust = [59, 24, 102]
for i in dust :
    print(i)
#딱 3번만 돌면서 요소를 1번씩만 접근
#i는 해당 하는 값을 저장
```

iteration은 **정해진 범위 안에서만** 한번씩 반복

while은 **조건을 충족**시킬 동안 계속 반복





### (2) 기본 개념

#### 1. keyword

- **keyword.kwlist** : python에서 기본적으로 사용되는 식별자를 출력



#### 2. 식별자

- 임의로 정한 이름에 변수를 할당
- 내장 함수를 식별자로 사용하면 안된다
- **del '변수'** : 혹여나 사용했을 경우 메모리 할당을 해제



#### 3. 내장 함수

- **str(i)** : i를 string형식으로 변환
- **type(i)** : i의 자료형 출력
- **id(i)** :  i의 메모리 주소를 출력

이 외에도 여러가지 내장함수가 존재



#### 4. 인코딩 선언

- **\# -*- coding: <인코딩 이름> -\*-** : python parser가 알아서 인코딩 수행



#### 5. 가독성

- **""" """** : 모두 주석처리
- **'함수'\__doc__**  : 함수의 주석만 출력
- **\\ **: 코드를 개행해서 작성 가능



#### 6. 변수 할당

- **a = b = 1** : 동시 할당
- **x, y = 1, 2** : 부분 할당
- swap : **x, y = y, x**
  - **미리 구현된 함수**이기 때문에 사용 가능
- 현재 컴퓨터의 메모리는 **0, 1이 들어가는 칸이 64개**(64 bit 체제)
  - 현재 운영체제의 **bit를 넘어가는 숫자는 표현이 불가**
  - 2^64 개의 숫자를 표현 가능
  - 넘어가면 overflow 발생
  - python은 **arbitrary-precision arithmetic**을 사용하여 이를 방지
- **0b2진수숫자** : 2진수 할당
- **0o8진수숫자** : 8진수 할당
- **0x16진수 숫자** : 16진수 할당





### (3) 자료형 표현

#### 1. 부동 소수점

- 소수점은 2진법을 통해 **근사값**으로 구현
- 따라서 정확한 수의 표현이 불가능 하다
- **round(숫자, 반올림 수)** : 반올림을 통해 정확한값 추출 가능

```python
0.1 + 0.2 == 0.3
>> False

round(0.1) + round(0.2) == 0.3
>> True
```



##### 두 수가 같은지 처리 방법

1. **절대값**으로 허용 오차범위 설정
2. **sys.float_info.epsilon**으로 절대값의 허용 오차범위 설정
3. **math.isclose(a, b)** : 두 수가 같은지(가까운지) 판별

```python
# python 내장 모듈 불러오기
import math, sys

# 1. 절대값 사용
def isSame(a, b):
  # 임의의 숫자 범위 이내
  if abs(a-b) < 0.0001:
    return True
  return False

# 2. sys.float_info.epsilon
def isSame(a, b):
  if abs(a-b) < sys.float_info.epsilon:
    return True
  return False

# 3. math.isclose(a,b)
def isSame(a, b):
  return math.isclose(a, b)
```



#### 3. 복소수 표현

- **j**로 허수 부분 표현
- **a.real** = 실수부
- **a.imag** = 허수부
- **a.conjugate** = 복소수 표현

```python
a = 3 - 4j
```



#### 4. None

- **값이 없음**을 의미하는 자료형. null







## III. 문자형 조작

- **\+** : 문자열 끼리 덧셈하여 이어붙이기 가능
- **f''' {i} '''** : i를 string과 함께 출력
- **""" """** : 개행 문자 없이 여러줄 출력
  - **"""{변수}"""** : String interpolation 가능
- **print('문자열', end = 'something')** : 뒤에 오는 print()문과 연결하여 표현
- **"%s" %문자열** : 문자열을 s에 출력
  - **"%d"** : 정수형 출력
  - **%(int, 'string')**으로 2개 출력
  - **%%** : '%' 출력
  - **%10s** : 앞에서 10칸 띄고 출력
  - **%-10s** : 뒤에서 10칸 띄고 출력
  - **%0.5f** : 소수점 5자리 까지 반올림 해서 출력
- **f"{변수}"** : 변수를 출력
  - fstring안에서 **연산**도 가능
- **'some'\*50** : 곱셈기호 사용해서 여러번도 출력 가능
- **.count('a')** : 문자열에서 'a'의 갯수를 출력
- **.find('a')** : 최초의 'a'가 몇번째에 있는지 출력
- **.upper()** : 소문자를 대문자로 출력

```python
print("good "*5)
>> good good good good good

print("good ", end="language")
>> good language
```



​	

#### 1. Slicing

- 문자열의 원하는 부분만 출력
- str[i] : i번째 문자 출력. 음수도 가능. 0을 첫번째 문자 기준으로.
- str[i:] : i 부터 문자열 끝까지
- str[:i] : 문자열 처음부터 i까지



#### 2. datetime

- 날짜 관련 라이브러리
- datetime.datetime.now() : 현재 날짜 시간 출력
- f'오늘은 {today:%y} 년 {today:%m}월 {today:%d}일 {today:%A}' 으로도 표현 가능







## III. 연산자

### 1. 기본 연산자

- divmod(5, 2)** : 몫과 나머지 출력
- **정수** : 0이 아니면 모두 참
- **정수의 and, or 연산자** : 연산이 참이면 앞 숫자, 거짓이면 뒷 숫자를 반환





### 2. 기타 연산자

- **Concatenation** : 숫자가 아닌 자료형을 **'+'** 연산자로 합칠 수 있다
  - list나 string을 합칠수도 있다
- **Containment test** : **in** 연산자로 속해있는지 여부를 판단할 수 있다
  - 문장 판단도 가능
- **Identity** : **is** 연산자로 동일한 object인지 판달할 수 있다
  - is : 객체가 저장된 **reference에 접근**하여 동일한 값인지 판단





### 3. 연산자 우선순위

1. `()`을 통한 grouping
2. Slicing
3. Indexing
4. 제곱연산자 **
5. 단항연산자 +, - (음수/양수 부호)
6. 산술연산자 *, /, %
7. 산술연산자 +, -
8. 비교연산자, `in`, `is`
9. `not`
10. `and`
11. `or`







## IV. 기초 형변환

#### 1. 암시적 형변환 

- 연산에 따라 python 내부에서 **자동으로 자료형 변환**이 발생
- bool, int = int

```python
True + 3
>> 4
```

- int, float = float

```python
3 + 4.5
>> 7.5
```

- int, complex = complex

```python
3 + (3 + 4j)
>> 6 + 4j
```



#### 2. 명시적 형변환

- 사용자가 직접 자료형 변환을 명시

```python
int(3.5)
# 데이터를 버리기 때문에 내림처리
>> 3
```



### 시퀀스 자료형

#### 1. List

- **[]**로 표현
- **[번호]** : 해당 번호 인덱스에 접근
- **mutable** : 수정 가능 객체
- del : 자료 제거



#### 2. Tuple

- 리스트와 유사
- **()**로 표현
  - 소괄호 안쓰고 s =  1, 2, 3, 4, 5 로도 할당 가능
- **immutable** : 수정 불가 객체
- x, y = 1, 2 또한 튜플



#### 3.range()

- 숫자의 **시퀀스**를 나타내기 위해 사용
- **range**라는 또 다른 자료형 사용
- range(n) : 0 ~ n-1까지 값을 지정
- range(n, m) : n ~ m-1 까지 값을 지정
- range(n, m, s) : n ~ m-1 까지 +s만큼 증가하면서 값을 지정



#### 4. 시퀀스에서 활용할 수 있는 연산자/함수

- in, not in
- '+'
- s * n : n번 **반복하여 붙이기**
- s[i:j]
- s[i]
- **len(s)** : 길이, **min(s)** : 최솟값, **max(s)** : 최댓값
- **s.count(x)** : s내 x의 갯수



#### 5. set

- **집합**과 동일한 내용
- **{}**로 표현
- **mutable**
- **중복된 값 불가**
  - 중복되면 무시함
- **index 접근 불가**. index의 의미가 없음
- **a \- b** : 차집합
- **a | b** = **a.union(b)** : 합집합
- **a & b** = **a.intersection(b)** : 교집합
- set을 활용하여 **list의 중복값을 제거**할 수 있다



#### 6. dictionary

- **key**, **value**가 쌍으로 구성된 자료구조
  - key나 value값으로 접근하기 때문에 **순서가 중요하지 않다**
- **{}**, **dict()**로 만들 수 있다
- **mutable**
- **len(dictionary)** : dictionary의 길이
- **value** : list, dictionary포함 모든것이 가능
  - **dictionary.values()**로 value값 확인 가능
- **key** : immutable한 모든것이 가능(string, integer, float, boolean, tuple, range...)
  - **dictionary['key']** : key로 value에 접근
  - key로 접근하여 **자료 수정** 가능
  - key로 접근하여 **자료 삽입** 가능
  - **key는 중복 불가**
  - **dictionary.keys()**로 key값 확인 가능


