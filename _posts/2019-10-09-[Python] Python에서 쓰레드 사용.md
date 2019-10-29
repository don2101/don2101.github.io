---
title: "Python에서 Thread 사용"
tags: ["Python", "Thread"]
---



파이썬에는 여러가지 종류가 있다

종류에 따라 스레드를 사용하는 방법이 다 다르며, 우리는 CPython을 기준으로 사용

<br>

## GIL(Global Interpreter Lock)

<hr>
- python에서 자원의 무결성과 동기화를 위한 기능
- 스레드는 동시성을 고도화 하기 위한 개념이며, 프로세스의 자원을 공유한다
  - 그러므로 자원의 무결성과 동기화를 위한 로직이 필요
<br>

### 1. 기본 개념

- CPython에서 여러 thread를 사용하는 경우 **하나의 thread만이 자원에 접근**하도록 제한하는 **mutex**
  - **mutex**: mutual exclusion(상호배제)
- CPython의 메모리 관리 방식이 thread-safe하지 않기 때문에 GIL을 사용
- **coarse-grained lock** 방식

<br>

#### coarse-grained lock

- 큰 단위로 lock을 잡는 방식
- 단일코어 CPU에서 사용
  - 락이 빈번하면 문맥교환이나 시간이 더 든다

<br>

#### find-grained

- 작은 단위로 락을 잡는 방식
- 멀티코어 CPU에서 사용
  - 여러 thread에서 lock을 걸 수 있다

<br>

### 2. 특징

- coarse-grained lock 방식
- 단일코어 CPU에서 성능을 발휘할 수 있다.

<br>

<br>

## Thread 구현

<hr>

### 1. Python에서 thread를 구현하는 방법

#### 저수준 라이브러리

- Thread pool이나 lock을 커스터마이징 할 수 있다
- thread원시 기능을 변경해서 사용 가능
- python2에서는 `thread`를 python3에서는 `_thread` 사용

<br>

#### 고수준 라이브러리

- 일반적으로 사용하며, 간편하게 사용 가능

<br>

### 2. 고수준 라이브러리 threading

#### 함수 구현

```python
import threading

def worker(count):
    # thread의 이름과 인자를 출력
    
    print("name: %s, argument: %s" %(threading.currentThread().getName(), count))

def main():
    for i in range(5):
        # target: thread로 실행할 함수의 이름
        
        # args: 함수의 인자를 tuple로 전달
        
        # name: thread의 이름
        
        t = threading.Thread(target=worker, name="thread %i" %i, args=(i,))
        t.start()

if __name__ == '__main__':
    main()
```

- `threading.Thread` 클래스에 thread로 **처리할 함수**를 넣고 초기화
- 초기화 할 때 쓰레드의 **이름**과 **매개변수**를 설정할 수 있다

<br>

> 결과

![](https://user-images.githubusercontent.com/19590371/66461043-1a221e80-eab3-11e9-8643-7a06df82e6e6.png)

- 스레드로 실행하면 비순차적으로 작업이 실행되어 실행 결과가 달라질 수 있다.
- CPU의 갯수가 1개이거나 코어가 1개라면 순차 실행이 된다

<br>

#### 클래스 메서드로 구현

```python
import threading

# threading.Thread 클래스를 상속

class Worker(threading.Thread):
    # 생성자 함수
    
    # Thread 클래스의 생성자를 호출
    
    def __init__(self, args, name=""):
        threading.Thread.__init__(self)
        self.args = args

    # 실제 실행할 함수(run)를 오버라이딩 하여 구현
    
    def run(self):
        # thread의 이름과 클래스의 속성을 출력
        
        print("name: %s, argument: %s" %(self.name, self.args[0]))
        
def main():
    for i in range(5):
        t = Worker(name="thread %i" %(i), args=(i, ))
        # 클래스 내부의 오버라이딩한 run을 실행
        
        t.start()

if __name__ == '__main__':
    main()
    
```

- `threading.Thread` 클래스를 상속받아 `run()`함수를 오버라이딩 하여 구현
- `threading.Thread` 의 생성자를 호출하지 않으면 모듈이 초기화 되지 않아 오류 발생
- `start()` 를 호출하면 내부의 `run()` 메서드 호출

<br>

> 결과

![](https://user-images.githubusercontent.com/19590371/66462085-4dfe4380-eab5-11e9-8c89-5b85db30c683.png)

<br>

### 3. logging

- python2에서는 thread에서 print사용시 안전하지 않고 출력도 불규칙하다
  - print문을 실행할 때 thread에 대해 별다른 조치 없이 실행
- **안전한 logging 모듈** 사용이 필요

<br>

#### thread에서 안전한 logging 구현

```python
import logging
import threading

# logging 모듈 설정

logging.basicConfig(level=logging.DEBUG, format="name: %(threadName)s argument: %(message)s")

def worker(count):
    logging.debug(count)

def main():
    for i in range(5):
        t = threading.Thread(target=worker, name="thread %i" %i, args=(i,))
        t.start()

if __name__ == '__main__':
    main()
    
```



### 4. Daemon Thread

- 쓰레드를 데몬으로 사용할 수 있다.
- 백그라운드에서 실행할 스레드를 데몬으로 띄워서 동작
- 스레드에 접근해서 조작을 할 수 없다.

<br>

#### 그렇다면 왜 데몬으로 사용?

- 그냥 스레드를 띄우면, 해당 스레드가 종료될 때 까지 기다린다.
  - 즉, 메인프로그램이 종료되지 않음
- **정기적**이고, **부수적**인 작업을 데몬 스레드로 띄운다.
  - 메인 프로그램 종료시 같이 종료

<br>

##### 데몬 스레딩 예시

```python
import time
import logging
import threading

logging.basicConfig(level=logging.DEBUG, format="(%(threadName)s) %(message)s")

def daemon():
    logging.debug("start")
    time.sleep(5)
    logging.debug("Exit")


def main():
    t = threading.Thread(name="daemon", target=daemon)
    # 스레드를 데몬으로 설정
    
    t.setDaemon(True)

    t.start()

if __name__ == "__main__":
    main()
```

<br>

> 결과

![](https://user-images.githubusercontent.com/19590371/67760760-3fd48f00-fa85-11e9-8b0a-107410d29bcd.PNG)

- 예상한 결과를 5초 뒤에 Exit 또한 같이 띄우는 것인데 start만 출력하고 종료되었다.
- 메인 프로그램이 종료되며 스레드도 기다리지 않고 같이 종료

<br>

#### 데몬 스레드를 기다리게 하는 방법

```python
t.setDaemon(True)

t.start()
# join을 추가

t.join()    
```

- 스레드의 `.join()`은 해당 스레드가 끝날 때 까지 기다리는 것을 의미한다.

<br>

> 결과

![](https://user-images.githubusercontent.com/19590371/67761013-c5583f00-fa85-11e9-8172-a4e3a4b1903a.PNG)

<br>