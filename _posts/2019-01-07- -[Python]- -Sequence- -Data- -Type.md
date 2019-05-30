---
title : "Sequence Data Type"
tags: ["Python"]
---

# Sequence Data Type

python에서 기본적으로 제공하는, 데이터를 연속적으로 저장하는 자료형

Sequence Data Type은 기본적으로 제공하는 메서드와, 적용할 수 있는 메서드가 많으므로 따로 분류해서 알아봅시다.

<br>

<br>

## I. Methods

### (1) List

#### list에 값을 넣고 빼는 방법

list는 정해진 공간 안에서 값을 수정할수 있기 때문에, 값을 추가하기 위해서 크기가 더 큰 **새로운 공간**을 할당 받아야 합니다.

```python
list1 = [1, 2, 3, 4, 5]
list2 = [6, 7, 8, 9, 10]
```

<br>

**`list`[len(`list`):]** : slicing으로 length 이후의 **공간을 추가**하는 방식 

```python
# list1에 공간을 만들고 새로운 list를 추가한다.
list1[len(list1):] = list2
print(list1)
>> [1, 2, 3, 4, 5, 6]
```

<br>

**`list`.extend(`list`)** : 리스트를 한번에 추가. **원본을 수정**한다.

```python
list1.extends([6])
print(list1)
>> [1, 2, 3, 4, 5, 6]
```

<br>

**'+'** : concatenation의 경우 **원본을 수정하지 않는다**.

```python
list3 = list1 + [6]
print(list3)
>> [1, 2, 3, 4, 5, 6]
```

<br>

**`list`.insert(i, x)** : `i`에 `x`를 추가. 범위를 벗어나거나 `i` 를 적지 않는 경우 맨 뒤에 추가

```python
list1.insert(3, 9)
# list 범위를 벗어나는 곳에 추가하면 맨 뒤에 추가
list1.insert(10, 10)
print(list1)
>> [1, 2, 3, 9, 4, 5, 10]
```

- python 내부적으로 새로운 공간을 할당한다.

<br>

**`list`.remove(`x`)** : 리스트에서 `x`를 하나 찾아 제거. `x` 가 없는 요소일 경우 에러 발생

```python
list1.remove(3)
print(list1)
>> [1, 2, 4, 5]

# 숫자가 존재하지 않을 경우 에러발생
list1.remove(6)
>> list1.remove(6)
ValueError: list.remove(x): x not in list
```

<br>

**`list`.pop(i)** : 정해진 위치의 값을 삭제 후 **반환**. `i` 를 적지 않을 경우 맨 뒤의 요소를 제거하여 반환

```python
num = list1.pop(2)
print(list1)
print(num)
>> [1, 2, 4, 5]
3
```

<br>

<br>

### (2) set

#### set에 값을 넣고 추가하는 방법

```python
set1 = {1, 2, 3, 4, 5}
```

<br>

**`set`.add(`x`)** : `x` 추가

```python
set1.add(6)
print(set1)
>> set([1, 2, 3, 4, 5, 6])
```

<br>

**`set`.update(`iterable`)** : 여러가지 값, set 순차적으로 추가. iterable 객체만 가능

```python
list1 = [3, 4, 5, 6, 7]
set1.update(list1)
print(set1)
>> set([1, 2, 3, 4, 5, 6, 7])
```

<br>

**`set`.remove(`x`)** : `x` 제거. `x`가 없는 요소일시 에러발생

```python
set1.remove(3)
print(set1)
>> set([1, 2, 4, 5])

set1.remove(6)
print(set1)
# 없는 요소 제거시 에러 발생
>> set1.remove(6)
KeyError: 6
```

<br>

**`set`.discard(`x`)** : `x` 제거. 없어도 에러 발생하지 않음

```python
set1.discard(6)
print(set1)
>> set([1, 2, 3, 4, 5])
```

<br>

**`set`.pop()** : `set` 의 맨 앞의 원소 제거해 반환

```python
num = set1.pop()
print(num)
print(set1)
>> 1
set([2, 3, 4, 5])
```

<br>

<br>

### (3) dictionary

#### dictionary 탐색

```python
dict1 = {
    1: "one",
    2: "two",
    3: "three",
}
```

<br>

**`dictionary`.values()**: dictionary의 모든 value값 **반환**

**`dictionary`.keys()**: dictionary의 모든 key값 **반환**

**`dictionary`.items()**: dictionary의 모든 key, value를 **tuple**로 반환

```python
# .values()
print(dict1.values())
>> ['one', 'two', 'three']

# .keys()
print(dict1.keys())
>> [1, 2, 3]

# .items()
print(dict1.items())
>> [(1, 'one'), (2, 'two'), (3, 'three')]
```

<br>

**`dictionary`.get(`key`)** : `key`에 해당하는  value값 반환. `key` 가 존재하지 않을 경우 None을 return.

**`dictionary`[`key`]**: `key`에 해당하는  value값 반환. `key` 가 존재하지 않을 경우 error 발생.

```python
print(dict1.get(1))
>> one

print(dict1.get(4))
>> None

print(dict1[1])
>> one

print(dict1[4])
>> print(dict1[4])
KeyError: 4
```

<br>

#### dictionary에 값을 추가 / 제거하는 방법

**`dictionary`[`key`] = `value`**: `key`에 `value`추가

```python
dict1[4] = "four"
dict1[5] = "five"
print(dict1)
>> {1: 'one', 2: 'two', 3: 'three', 4: 'four', 5: 'five'}
```



**`dictionary`.pop(key, default)** : `key`값으로 찾아 제거하고 value 반환. 없을 경우 **default** 반환

```python
print(dict1.pop(1))
>> one

print(dict1)
>> {2: 'two', 3: 'three'}

print(dict1.pop(4, None))
>> None
```

<br>

**`dictionary`.update(`dictionary`)** : 새로운 `dictionary` 를 추가

```python
dict2 = {
    4: "four",
    5: "five",
}
dict1.update(dict2)
print(dict1)
>> {1: 'one', 2: 'two', 3: 'three', 4: 'four', 5: 'five'}
```

<br>

<br>

### (4) String

#### string 변환 관련 함수

```python
str1 = 'c'
str2 = "hello world!"
str3 = "Good Day!"
```

<br>

**str(`object`):** 객체를 string 객체로 변환하여 반환

**ord(`character`):** 문자 하나를 ascii code로 변환하여 반환

**`string`.upper():** 모든 문자 대문자로 변환하여 반환.

**`string`.lower() :** 모든 문자 소문자로 변환하여 반환.

```python
# string()
print(str(12345))
>> "12345"

# ord()
print(ord(str1))
>> 99

# .upper()
print(str2.upper())
>> "HELLO WORLD!"

print(str3.lower())
>> "good day!""
```

- 모두 기존 객체 보존

<br>

**.replace(old, new, [ count])**: String 내부에 old가 있을경우 new로 변환 후 **반환**. count 갯수 만큼 시행

```python
print(str2.replace('l', 'p', 2))
>> "heppo world!"
```

- 기존객체 보존
- 없으면 아무것도 하지 않음

<br>

**`string`.capitalize()** : 앞글자를 대문자로 만들어 **반환**

**`string`.title()** : 어포스트로피나 공백 바로 이후 글자를 대문자로 만들어 **반환**

**`string`.swapcase()** : 대문자를 소문자로 소문자를 대문자로 변환 후 **반환**

```python
# .capitalize()
>> "Hello world!"

# .swapcase()
>> "gOOD dAY!"
```

<br>

#### String 조작 관련 함수

**.strip()** : 양쪽의 공백 문자들 제거

- **.lstrip()** : 왼쪽의 공백 문자들 제거
- **.rstrip()** : 오른쪽의 공백 문자들 제거

<br>

**"".join(`iterable`)** : list 객체를 String으로 변환하면서, 각 list 사이에 ""을 사이에 삽입하고 반환

```python
list1 = ["hello", "I'm", "a", "postman"]

# 사이에 공백 문자 추가
str4 = " ".join(list1)
print(str4)

>> "hello I'm a postman"
```

- 기존 객체 보존
- list의 모든 요소는 string객체여야 함

<br>

**`string`.split()** : 구분자(delimeter)를 기준으로 문자열을 구분하여 list로 반환

```python
print(str2.split())
>> ['hello', 'world!']
```

- 아무것도 넣지 않으면 **공백 문자 기준** 구분

<br>

**`string`.find(x)** : 문자열에서 처음으로 나오는 x의 위치 반환. 없으면 -1 **반환**

**`string`.index(x)** : find(x)와 같지만 없을 경우 error 발생

```python
# .find()
print(str2.find('l'))
print(str2.find('z'))
>> 2
>> -1

# .index()
print(str2.index('l'))
print(str2.index('z'))
>> 2
>> print(str2.index('z'))
ValueError: substring not found
```

<br>

**`string`.isdigit()**: 문자열이 숫자형태인지 **반환**

```python
str4 = "123"
print(str4.isdigit())
>> True
```

<br>

<br>

<br>

## II. 객체 복사

python에서 객체를 복사함에 있어 레퍼런스 복사와 값 복사로 나뉜다.

<br>

#### immutable 객체 복사

**값 자체**를 복사, 복제(string, integer...)

`=`복사를 한다고 해도 값 자체가 복사되기 때문에 각 변수에서 조작 가능

```python
num1 = 9
num2 = num1
num2 += 1

print(num1, num2)
>> (9, 10)
```

<br>

#### mutable 객체 복사

**레퍼런스**를 복사. 

`=`으로 복사할 경우 레퍼런스가 복제되기 때문에 두 변수가 접근해서 조작할 경우 **서로에게 영향**(레퍼런스 참조)

```python
list1 = [1, 2, 3]
list2 = list1
list1.append(4)
# list1에 값을 추가했지만, list2 또한 값이 추가됨
print(list2)
>> [1, 2, 3, 4]
```

<br>

#### 객체 비교

**is** : **레퍼런스값** 비교. 실제 동일한 객체?

<br>

#### 얕은 복사

**list1 = list2[:]** : slicing을 하여 값 자체를 복사 

**.copy()** : 객체의 값을 복사하여 반환

**b = list(a)** : list operator를 사용해서 복제

<br>

#### 깊은 복사

얕은 복사는 리스트 안의 리스트 등 **객체 안의 객체의 값은 복사하지 않는다**.

객체 안의 객체는 레퍼런스만 가져오며, 한 깊이만을 복사

import copy의 deepcopy로 객체 내 모든 객체에 대해 복사 가능

<br>

<br>

<br>

## III. 공통 조작

**iterable객체**(순회 가능한 객체, dictionary, list, set)에 대해 공통적으로 조작이 가능한 메서드

<br>

#### 공통 메서드

**`iterable`.sort()** : 정렬. 원본 리스트를 변형하고, **None을 반환**

**sorted(`iterable`):** 객체를 오름차순으로 정렬하여 list로 반환, True 옵션 추가시 내림차순 정렬

```python
print(sorted(list1))
>> [1, 2, 3, [1, 2, 3], 'a', 'b']

# reverse=True 옵션 추가시 내림차순 정렬
print(sorted(list1), reverse=True)
>> ['b', 'a', [1, 2, 3], 3, 2, 1]
```

- 기존 객체는 보존
- 원본 객체를 변형하지 않는 sorted가 더욱 선호됨

<br>

**`iterable`.reverse()** : 객체를 반대로 뒤집는다. 원본 객체 훼손.

**reversed(`iterable`):** 모든 요소를 거꾸로하여 반환

```python
print(list1)
>> ['a', 'b', 1, 2, 3, [1, 2, 3]]

print(list(reversed(list1)))
>> [[1, 2, 3], 3, 2, 1, 'b', 'a']
```

- **reversed 객체**가 반환되므로 list나 tuple로 바꿔서 출력할 수 있다. 
- **기존 객체는 보존**된다.

> palindrome 확인 방법

```python
if(reversed == str)
```

<br>

**len(`iterable`):** 길이 구해서 반환

```python
print(len(list1))
>> 6
```

<br>

**sum(`iterable`):** 모든 요소 더해서 반환

```python
print(sum(list2))
>> 15
```

- list의 모든 객체가 숫자일 경우 가능

<br>

**min(`iterable`)** : 최솟값, **max(`iterable`)** : 최댓값

**`iterable`.count(`x`)** : `iterable`내 `x`의 갯수

<br>

<br>

#### 공통 조작

**map(`function`, `iterable`)**: `object` 의 모든 요소에 `function` 을 적용하여 반환

```python
def addTwo(number) :
    return number+2

numList = [1, 2, 3, 4, 5]
result = map(addTwo, numList)
```

> input()에서 숫자들을 받기

```python
# 공백으로 구분된 입력을 숫자 리스트로 반환
map(int, input().split())
```

<br>

**zip(`iterable`)** : 동일한 개수로 이루어진 자료형을 묶어 tuple로 반환

```python
>>> list(zip([1, 2, 3], [4, 5, 6]))
[(1, 4), (2, 5), (3, 6)]
>>> list(zip([1, 2, 3], [4, 5, 6], [7, 8, 9]))
[(1, 4, 7), (2, 5, 8), (3, 6, 9)]
>>> list(zip("abc", "def"))
[('a', 'd'), ('b', 'e'), ('c', 'f')]
```

<br>

**filter(`function`, `object`)** : `object`에서 **`function` 결과가 참 인것만** 구성하여 반환

```python
a = [1, 2, 3]
list(filter(even, a))
```