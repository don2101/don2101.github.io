# 자료형

python에서 기본적으로 제공하는, 자료를 저장하기 위한 객체

수정가능한 **mutable**, 수정이 불가능한 **immutable** 객체로 구분 가능

<br>

## I. mutable vs immutable?

값이 변할 수 있다는 것과 값이 변할 수 없다는 것은 어떤의미일까요?

<br>

python에서 모든 자료는 객체입니다. c나 java같은 경우 int나 char등의 자료형으로 저장하기도 하지만, python의 경우 모든 자료를 객체로 저장합니다.

따라서 같은 값이라도 다른 객체(메모리에서 다른 주소값을 가르키는 객체)일 수 있습니다.

<br>

<br>

<br>

## II. immutable

변경 불가능 하다는것은 실행단계에서 **자료가 변할 수 없다**는 것을 의미합니다.

예를들어 python에서 integer는 immutable합니다. 우리가 일반적으로 a=1 이라고 선언하는 것이 a에 1이라는 값을 넣는게 아닌 **1이라는 값을 가진 integer 객체**가 들어가는 것입니다.

<br>

<br>

### (1) immutable 객체의 특성

다음의 예를 봅시다.

```python
a = 1 # a에 1의 값을 가진 integer객체 할당. a가 그 객체를 가리킨다.
a = 2 # a에 2의 값을 가진 integer객체 할당. a는 새로운 객체를 가리킨다.
```

일반적으로 a의 값이 1에서 2로 변경됐다고 생각할 수 있지만, integer객체는 immutable하다고 했습니다.

실제로 a가 **가리키는 객체가 변경**된 것이지, **값이 변경된 것이 아닙니다**.

<br>

string또한 immutable합니다.

```python
a = "hello"
a = "hello!"
```

실제로 위의 경우 a에 "!"가 추가된 것이 아닌 "hello"에서 "hello!"라는 새로운 객체를 가리키게 되는 것입니다. 절대로 **값이 변경된 것이 아닙니다**.

<br>

<br>

### (2) Integer & float

- python에서 숫자를 저장하는 자료형. integer는 정수를, float는 실수를 저장.

- 기본적으로 사칙연산이 가능합니다.

<br>

#### 특이한 연산자

```python
# 지수곱
2 ** 4
>> 16

# 나머지를 버리는 나눗셈
5 // 2
>> 2

# modular 연산
17 % 5
>> 2
```

<br>

#### 관련 함수

> int(): integer형으로 변환

```python
a = "12"
b = int(a) # b에 숫자 12가 대입
print(b)
>> 12
```

- 정수형이 아닐 경우 에러 발생

<br>

<br>

### (3) tuple

- 리스트와 유사하나, 자료를 삭제할 수 없는 자료형
- `( )` 안에 자료를 표현
  - 소괄호 안쓰고 s =  1, 2, 3, 4, 5 로도 할당 가능
- x, y = 1, 2 또한 튜플

##### 선언 방법

```python
# tuple 선언
a = (1, 2, 3)
a = 1, 2, 3, 4, 5

# 여려 변수로 선언
x, y, z = "hello", 100, "world!"

# 이후 변수로 데이터 접근 가능
print(x, z)
>> ('hello', 'world!')
```



> 더하기가 가능하다

```python
t1 = (1, 2, 3)
t2 = (3, 4, 5)
t1 += t2

print(t1)
>> (1, 2, 3, 3, 4, 5)
```





<br>

#### 3. string

- 문자 혹은 문자열을 저장하는 객체
- `' '` 혹은 `" "` 안에 자료를 선언

```python
str1 = 'c'
str2 = "hello world!"
```

- ord('a') : ascii code 변환
- 'char'.upper() : 문자 대문자로 변환
- 'char'.lower() : 소문자로 변환





- .replace(old, new, [ count])** : String 내부에 old가 있을경우 new로 변환 후 **반환**. count 갯수 만큼 시행
- **.capitalize()** : 앞글자를 대문자로 만들어 **반환**
- **.title()** : 어포스트로피나 공백 바로 이후 글자를 대문자로 만들어 **반환**
- **.upper()** : 모두 대문자로 만들어 **반환**
  - **.lower()** : 모두 소문자로
- **.swapcase()** : 대문자를 소문자로 소문자를 대문자로 변환 후 **반환**





#### String 조작

- **.strip()** : 양쪽의 공백 문자들 제거
  - **.lstrip()** : 왼쪽의 공백 문자들 제거
  - **.rstrip()** : 오른쪽의 공백 문자들 제거
- **"".join(iterable)** : 리스트 객체를 String으로 변환. " "을 사이에 삽입
  - 리스트의 자료들은 반드시 String이어야 함
- **map(자료형, 문자 리스트)** : 문자 리스트를 자료형으로 변환하여 리스트로 **반환**
- **split()** : 구분자(delimeter)를 기준으로 문자를 리스트로 구분
- **.find(x)** : 문자열에서 처음으로 나오는 x의 위치 반환. 없으면 -1 **반환**
  - **.index(x)** : find(x)와 같지만 없을 경우 error 발생



#### 2. List

- **[]**로 표현
- **[번호]** : 해당 번호 인덱스에 접근
- **mutable** : 수정 가능 객체
- del : 자료 제거



- 



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