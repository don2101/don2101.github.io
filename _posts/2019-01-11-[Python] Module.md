---
title: Python Module
tags: ["Python"]
---



# Module

**import** : 다른 파일에 있는 **코드를 가져와** 사용한다.

- 코드가 어디있든, 내가 만들었든지 간에 코드를 가져온다.

<br>

파이선은 **컴파일 언어가 아니다!**

**시작점(main)**을 잡기가 어렵다

```python
if __name__ == "__main__":
```

- 자신이 시작점일 때만 main 함수로 간주
- **\__name__** : 에는 **파일의 이름** 혹은 **\__main__** 이 담겨있다**. 실행되는 컨텍스트**가 들어있다.(main 혹은 module)

<br>

```python
from module import add_two, three, four
```

- **from \<클래스, 파일> import \<모듈>** : \<클래스>에 있는 \<모듈>을 가져온다
  - 코드를 가져와서 실행시 가져온 **모듈을 메모리 공간**에 올림
- **dir(\<클래스>)** : \<클래스> 의 명세 출력

<br>

<br>

## 기타 모듈

### math

**sum, max, min, abs, pow, round, divmod** : import 하지 않고 사용 가능. 내장 함수

**math.ceil(x)** : 소수점 올림

**math.floor(x)** : 소수점 내림

<br>

### datetime

**datetime.datetime.now()** : 현재 날짜, 시간 = .today()

- **.day, .month, .year** = 일, 월, 년
- **.weekday()** : 0부터 월요일

**datetime.datetime.utcnow()** : utc 기준 시간

**datetime.datetime(y, m, d...), datetime.date(y, m, d...)** : 특정 날짜 만들기

- **.strftime("%Y %m %d")** : 정리해서 출력

<br>

### timedelta

시간 차이 구하기

```python
ago = timedelta(days=-3)
```

