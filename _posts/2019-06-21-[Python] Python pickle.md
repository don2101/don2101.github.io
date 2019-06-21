---
title: "pickle"
tags: ["Python", "Serialization"]
---



# Python pickle

##### 2019. 06. 21 (Fri)

<br>

## pickle

<hr>

객체를 직렬화, 역직렬화하기 위해 binary 프로토콜로 변환하는 기능을 가진 라이브러리

클래스나 딕셔너리등의 데이터를 저장하고 불러올 때 유용하다.

이러한 객체를 그대로 저장하면 용량이 매우 커지지만, pickle을 사용하면 binary 형태로 저장하기 때문에 용량이 작아진다.

<br>

#### 직렬화?

객체를 바이트 스트림으로 변환하는 과정

저장을 효율적으로 하는데 사용할 수 있으며, 객체를 바이트 스트림으로 변환하여 전송 가능하게 만들 수 있다.

<br>

<br>

### 1. pickle화 될 수 있는 자료형들

- `None`, `True` 와 `False`
- 정수, 실수, 복소수
- 문자열, 바이트열, 바이트 배열(bytearray)
- 피클 가능한 객체만 포함하는 튜플, 리스트, 집합과 딕셔너리
- 모듈의 최상위 수준에서 정의된 함수 (`lambda` 가 아니라 `def` 를 사용하는)
- 모듈의 최상위 수준에서 정의된 내장 함수
- 모듈의 최상위 수준에서 정의된 클래스

<br>

##### 예시 코드

```python
import pickle

data = {
    'a': [1, 2, 3, 4, 5],
    'b': 'string',
    'c': {6, 7, 8, 9, 10},
}

class Example:
    def __init__(self):
        self.one = 1
        self.two = 2

        
with open('data.pickle', 'wb') as f:
    pickle.dump(data, f)
    pickle.dump(Example, f)


with open('data.pickle', 'rb') as f:
    data = pickle.load(f)
    print(data)
    data = pickle.load(f)
    print(data)
```

<br>

<br>

<br>

## 다른 모듈과의 비교

<hr>

### 1. pickle vs marshal

marshal또한 python에서 지원하는 직렬화 모듈이지만, marshal은 주로 `.pyc`파일을 지원하기 위해 존재한다.

또한, marshal은 사용자 정의 클래스와 인스턴스를 직렬화 하는데 사용할 수 없으며, python 버전간 이식성이 보장되지 않는다.

<br>

|                         | pickle   | marshal          |
| ----------------------- | -------- | ---------------- |
| 지원 모듈               | 직렬화   | .pyc             |
| 클래스 직렬화 가능 여부 | 가능     | 불가능           |
| 버전간 이식성           | 보장한다 | 보장하지 않는다. |

<br>

<br>

### 2. pickle vs Json

Json은 텍스트 직렬화 형식이며, pickle은 binary 직렬화 형식이다.

Json은 사람이 읽을 수 있는 자료로 변환한다.

Json은 python 내장형 자료형만 표시할 수 있으며, 사용자 정의 클래스 등은 표시할 수 없다.

<br>

|                       | pickle                   | Json                   |
| --------------------- | ------------------------ | ---------------------- |
| 직렬화 형식           | 유니코드 텍스트          | 바이너리               |
| 사람이 읽을 수 있는가 | 가능                     | 불가능                 |
| 운용성                | python 외부에서 사용가능 | python 내부에서만 사용 |
| 지원하는 자료형       | python 내장 자료형       | + 사용자 정의 클래스   |







### 참고자료

<https://wikidocs.net/8929>

<https://docs.python.org/ko/3/library/pickle.html>