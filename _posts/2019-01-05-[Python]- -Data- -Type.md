# 자료형

python에서 기본적으로 제공하는, 자료를 저장하기 위한 객체

수정가능한 **mutable**, 수정이 불가능한 **immutable** 객체로 구분 가능

<br>

## I. mutable vs immutable?

---

값이 변할 수 있다는 것과 값이 변할 수 없다는 것은 어떤의미일까요?

<br>

python에서 모든 자료는 객체입니다. c나 java같은 경우 int나 char등의 자료형으로 저장하기도 하지만, python의 경우 모든 자료를 객체로 저장합니다.

우리가 일반적으로 a=1 이라고 선언하는 것이 a에 1이라는 값을 넣는게 아닌 **1이라는 값을 가진 integer 객체**가 들어가는 것입니다.

따라서 같은 값이라도 다른 객체(메모리에서 다른 주소값을 가르키는 객체)일 수 있습니다.

<br>

<br>

<br>

## II. mutable

---

mutable객체는 실행 단계에서 객체 내부의 값을 **수정 할 수 있습니다.**

immutable에 속하는 list나 dictionary같은 객체들은 실행 단계에서 그 내부의 값을 언제든지 수정할 수 있습니다. 

<br>

### (1) List

데이터를 순서 있게 나열한 자료형

`[]`로 선언

list안에 다른 자료형 선언 가능

```python
# list선언, 다른 자료형도 같이 추가 가능
list1 = ['a', 'b', 1, 2, 3, [1, 2, 3]]
list2 = [1, 2, 3, 4, 5]
```

<br>

> Index 접근 가능

```python
print(list1[2])
>> 1
```

<br>

> mutable객체이기 때문에 index값을 수정 가능

```python
list1[2] = 5
print(list1)
>> ['a', 'b', 5, 2, 3, [1, 2, 3]]
```

- 수정한 내용은 객체에 적용됩니다.

<br>

> list끼리 더하기

```python
list3 = list1 + list2
print(list3)
>> ['a', 'b', 5, 2, 3, [1, 2, 3], 1, 2, 3, 4, 5]
```

- 더한 순서대로 뒤에 이어붙입니다.

<br>

#### 관련 함수

**list(`iterable`):** iterable 객체를 tuple로 변환

<br>

<br>

<br>

### (2) set

**집합**과 동일한 개념

**{}**로 선언

**중복된 값 불가**

- 중복되면 무시함

**index 접근 불가**

- 순서가 없는 객체이기 때문에 index의 의미가 없음

```python
set1 = {1, 2, 3, 1, 2}
set2 = {1, 2, 4, 5}
print(set1)
# 중복된 값 무시
>> set([1, 2, 3])

# index 접근 시도
print(set1[0])
>> print(set1[0])
TypeError: 'set' object does not support indexing
```

<br>

#### 연산자

**a \- b** : 차집합하여 반환

**a | b** = **a.union(b)**: 합집합하여 반환

**a & b** = **a.intersection(b)**: 교집합하여 반환

set을 활용하여 **list의 중복값을 제거**할 수 있다

```python
# 차집합
print(set1 - set2)
>> set([3])

# 합집합
print(set1.union(set2))
>> set([1, 2, 3, 4, 5])

# 교집합
print(set1.intersection(set2))
>> set([1, 2])
```

- 모든 연산자는 원본 객체를 보존

<br>

<br>

### (3) dictionary

**key**, **value**가 1:1 쌍으로 이루어진 자료형

- key나 value값으로 접근하기 때문에 **순서가 중요하지 않다**

**{}**, **dict()**로 선언할 수 있다.

value는 중복이 될 수 있지만, key는 중복이 될 수 없다.

key값을 통해 value에 접근하고 수정할 수 있다.

```python
dict1 = {
  "fruit1": "apple",
  "fruit2": "grape",
	# key값으로 숫자도 넣을 수 있다.
  1: "one",
  "two": 2,
}

# key값을 통해 접근
print(dict1["fruit1"])
print(dict1["fruit2"])
print(dict1[1])
print(dict1["two"])

# key값을 통해 value 수정
dict1["two"] = "two"
print(dict1["two"])
>> "two"
```

<br>

<br>

<br>

## III. immutable

---

변경 불가능 하다는것은 실행단계에서 **자료가 변할 수 없다**는 것을 의미합니다.

immutable에 속하는 set이나 string 객체가 새로운 객체를 가리킬 수는 있지만, 그 내부의 값을 변경할 수 없습니다.

<br>

<br>

### (1) immutable 객체의 특성

다음의 예를 봅시다.

```python
a = 1 # a에 1의 값을 가진 integer객체 할당. a가 그 객체를 가리킨다.
a = 2 # a에 2의 값을 가진 integer객체 할당. a는 새로운 객체를 가리킨다.
```

일반적으로 a의 값이 1에서 2로 변경됐다고 생각할 수 있지만, integer객체는 immutable하다고 했습니다.

실제로 a가 **가리키는 객체가 변경**된 것이지, **a의 값이 변경된 것이 아닙니다**.

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

##### int(): 정수형으로 변환

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
- 변수에 매칭시켜서 데이터에 접근이 가능하다.

> 선언 방법

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

<br>

> tuple끼리 더하기

```python
t1 = (1, 2, 3)
t2 = (3, 4, 5)
t1 += t2

print(t1)
>> (1, 2, 3, 3, 4, 5)
```

<br>

> 인덱스 접근 가능

```python
print(t1[0])
>> 1
```

<br>

> immutable객체이기 때문에 특정 인덱스 수정은 불가

```python
t1[0] = 2
>> t1[0] = 0
TypeError: 'tuple' object does not support item assignment
```

<br>

#### 관련 함수

**tuple(`iterable`):** iterable 객체를 tuple로 변환

<br>

<br>

### (4) string

- 문자 혹은 문자열을 저장하는 객체
- `' '` 혹은 `" "` 안에 자료를 선언

```python
str1 = 'c'
str2 = "hello world!"
str3 = "Good Day!"
```

<br>

> index접근 가능

```python
print(str2[0])
>> 'h'
```

<br>

> immutable객체 이기 때문에 특정 인덱스 수정은 불가

```python
str1[0] = 'a'
>> str1[0] = 'a'
TypeError: 'str' object does not support item assignment
```

<br>
