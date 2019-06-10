---
title: "Python Unit Test"
tags: ["Python", "Unit Test"]
---



# Python Unit Test

#### Unit test(단위 테스트)?

모듈 또는 프로그램 내의 코드 단위가 예상대로 작동하는지 확인하는 작업

소프트웨어를 개발하면서 어떤 기능 하나 혹은 코드가 제대로 작동하는지 확인한다

<br>

<br>

## I. unittest

<hr>

다양한 테스트를 자동화 하여 사용할 수 있는 Python 내장 라이브러리

<br>

### 1. unittest 기본 개념

#### TestCase

unittest 라이브러리의 기본 테스트 단위

<br>

#### Fixture

테스트함수의 전 또는 후에 실행하는 기능

setUp(): 테스트를 실행하기 전 변수 및 환결설정을 하는데 사용

tearDown(): 테스트 후에 사용한 리소스를 해제하거나 정리하는데 사용

두 함수는 테스트 실행시 자동으로 실행

<br>

#### assertion

정의한 테스트의 실행결과가 예상한 결과에 포함되는지 검정

assertion이 실패하면 테스트 함수가 실패한다

실행 결과가 같다`assertEqual`, 다르다`assertNotEqual` 등이 존재

unittest 내부 메서드이므로 `self.` 로 불러와 사용한다

[assert 메소드 리스트](https://wikidocs.net/16107)

<br>

<br>

### 2. unittest 사용

1. 테스트할 기능 작성
2. `unittest`를 import하고, `unittest.TestCase`를 상속한 클래스 선언
3. 선언한 클래스 안에 실행할 test를 `test_` 를 붙여서 정의
4. unittest.main()으로 테스트를 실행

<br>

#### 작동 되는지만 검사하기

##### calc.py

```python
def add(a, b):
    return a + b

def substrat(a, b):
    return a - b
```

<br>

##### unit_test.py

```python
import unittest
import calc


class CalcTest(unittest.TestCase):
    def test_add(self):
        calc.add(10, 20)


    def test_substract(self):
        calc.substract(20, 10)

unittest.main()
```

<br>

##### 결과

```bash
..
----------------------------------------------------------------------
Ran 2 tests in 0.000s

OK
```

- 두 함수가 모두 성공적으로 실행된다.

<br>

#### 실행 결과를 검정하기

##### unit_test.py

```python
import unittest
import calc


class CalcTest(unittest.TestCase):
    def test_add(self):
        result = calc.add(10, 20)
        # calc.add 실행 결과가 30인지 검정
        self.assertEqual(result, 30)


    def test_substract(self):
        result = calc.substract(20, 10)
        # calc.substract 실행 결과가 0~50 사이에 존재하는지 검정
        self.assertIn(result, range(0, 50))

unittest.main()
```

<br>

```bash
..
----------------------------------------------------------------------
Ran 2 tests in 0.000s

OK
```

<br>

##### 만약 result != 35로 설정하여 테스트에 실패할 경우

```python
F.
======================================================================
FAIL: test_add (__main__.CalcTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "unit_test.py", line 8, in test_add
    self.assertEqual(result, 35)
AssertionError: 30 != 35

----------------------------------------------------------------------
Ran 2 tests in 0.000s

FAILED (failures=1)
```

<br>

#### Fixture 사용

##### unit_test.py

```python
import unittest
import calc


class CalcTest(unittest.TestCase):
    def setUp(self):
        # input으로 데이터를 설정하는 것도 가능
        self.pre_data = input()
        

    def test_add(self):
        result = calc.add(self.pre_data, 20)
        self.assertEqual(result, 35)


    def test_substract(self):
        result = calc.substract(self.pre_data, 10)
        self.assertIn(result, range(0, 50))


unittest.main()
```





