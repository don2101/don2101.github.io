# Sequence Data Type

python에서 기본적으로 제공하는, 데이터를 연속적으로 저장하는 자료형

Sequence Data Type은 기본적으로 제공하는 메서드와, 적용할 수 있는 메서드가 많으므로 따로 분류해서 알아봅시다.

<br>

<br>

## I. Methods

### (1) List

#### list에 값을 넣고 빼는 메서드

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















#### 공통 조작

- **.sort()** : 정렬. 원본 리스트를 변형하고, **None을 반환**
- **sorted(리스트)** : 원본 리스트를 **변형하지 않고**, 정렬된 리스트를 **반환**
  - 보통 원본을 훼손하지 않는 방법이 선호됨
- **.reverse()** : 리스트를 반대로 뒤집고, **None을 반환**
- **reversed(**) : **원본 훼손하지 않음**. reversed object. 리스트가 아님



**list(`iterable`):** iterable 객체를 tuple로 변환

<br>

**len(`list`):** 길이 구해서 반환

```python
print(len(list1))
>> 6
```

<br>

**sum(`list`):** 모든 요소 더해서 반환

```python
print(sum(list2))
>> 15
```

- list의 모든 객체가 숫자일 경우 가능

<br>

**reversed(`list`):** 모든 요소를 거꾸로하여 반환

```python
print(list1)
>> ['a', 'b', 1, 2, 3, [1, 2, 3]]

print(list(reversed(list1)))
>> [[1, 2, 3], 3, 2, 1, 'b', 'a']
```

- **reversed 객체**가 반환되므로 list나 tuple로 바꿔서 출력할 수 있다. 
- **기존 객체는 보존**된다.

<br>

**sorted(`list`):** tuple을 오름차순으로 정렬하여 list로 반환

```python
print(sorted(list1))
>> [1, 2, 3, [1, 2, 3], 'a', 'b']

# reverse=True 옵션 추가시 내림차순 정렬
print(sorted(list1), reverse=True)
>> ['b', 'a', [1, 2, 3], 3, 2, 1]
```

- 기존 객체는 보존