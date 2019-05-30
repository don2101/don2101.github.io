---
title : "Python Function"
tags: ["Python"]
---

# Python Function

- 코드가 많아질수록 문제 발생 확률이 높아지며, 유지보수도 힘들어진다.

<br>

## I. 함수

### 활용법

`def`로 정의

매개변수를 받아 하나의 값을 반환

- 리스트를 통해 여러개 반환도 가능

4칸 indentation 필요

```python
def func(param1, param2) :
    code line1
    code line2
    return value
```

<br>

<br>

### 함수의 return

함수값의 반환이 있을 수 있다.

어떤 종류의 객체도 반환이 된다.

- 함수 또한 가능

<br>

<br>

### 기본값(Default Argument Values)

```python
def func(p1 = v1) :
    return p1
```

기본값이 있는 매개변수는 **맨 뒤에 작성**해야 한다.

- 기본 매개변수 이후에 기본 값이 없는 매개변수는 사용할 수 없다.

호출시 값을 설정하지 않아도 설정한 값으로 적용된다.

<br>

> print()는?

print(*objects, sep='', end='\n', file=sys.stdout, flush=False)함수 또한 기본값들이 설정되어 있다.

- sep 으로 구분
- end를 뒤에 부임
- flush = True이면 스트림이 강제로 플러시됨

<br>

<br>

### 가변 인자 리스트

- 정해지지 않은 수의 인자를 받기 위해 가변 인자 사용
- 가변인자는 **tuple형태**로 처리
- `*`로 표현

```python
def exFunc(*param) :
```

<br>

<br>

### 정의되지 않은 인자들 처리하기

`**`로 표현

**dictionary 형태**로 처리

보통 **kwagrs**라는 이름을 사용

```python
def func(**kwargs) :
```

정의되지 않은 인자들은 dictionary로 처리한다

함수 호출시  `**`을 붙여 dictionary를 전달할 수있다.

<br>

> 매개변수로 받을 때 

```python
def user(userName, password, password_confirm) :
    if password == password_confirm :
        print(f"{userName} 회원가입 완료")
    else :
        print('비밀번호와 비밀번호 확인이 일치하지 않습니다.')
        
my_account = {
    'userName' : 'kim',
    'password' : '12345',
    'password_confirm' : '12345'
}

user(**my_account)
```

```python
# **로 인자를 받는다.
def user(**kwargs) :
  	# dictionary 형태로 인자에 접근
    if kwargs['password'] == kwargs['password_confirm'] :
        print(f"{kwargs['userName']} 회원가입 완료")
    else :
        print('비밀번호와 비밀번호 확인이 일치하지 않습니다.')
        
my_account = {
    'userName' : 'kim',
    'password' : '12345',
    'password_confirm' : '12345'
}

user(**my_account)
```

<br>

<br>

### 함수를 정의하는 방법

여러가지 기능을 어떻게 분리할지 생각

1. 함수는 기능을 모듈화(분리)
2. 입력을 무엇을 받을지 생각
3. 결과가 어떻게 되는지 생각
4. 입력 에서 결과가 도출되는 과정을 생각

변수 정의중 vs code에서 노란색으로 표시되면 예약어라는 것을 의미

<br>

<br>

<br>

## II. 이름공간(namespace)과 스코프(scope)

#### 이름공간

파이선에서 사용되는 변수은 이름공간에 저장되어 있다.

밖에 있는 이름공간은 안에 있는 이름공간에 접근 불가

안에 있는 이름공간은 밖에 있는 이름공간에 접근 가능

<br>

스코프 : LEBG Rule에 따라 변수 범위 적용. L부터 접근하여 적용한다.

- `L`ocal scope: 정의된 함수 내부
- `E`nclosed scope: 상위 함수
- `G`lobal scope: 함수 밖의 변수 혹은 import된 모듈
- `B`uilt-in scope: 파이썬안에 내장되어 있는 함수 또는 속성

```python
  str = '4' #내장함수 str을 변수로 사용
  str(3)
```

```python
# 함수 내부에서 전역변수 a에 접근할 수 없다
a = 1
def localScope(a) :
    print(a)

localScope(3)
##################################
a = 1
def localScope(a) :
    a = 5
    print(a)

localScope(5)
print(a)
```



