---
title: "Thread의 lock"
tags: ["Python", "Thread"]
---



## Thread Lock

<hr>

#### 스레드에서 자원을 사용할 때

- 서로 다른 스레드가 동시에 자원에 접근하면 **무결성**이 훼손될 수 있다.
- 스레드를 사용할 때는 자원에 대한 무결성을 보장해야 한다.
- Python 내장 자료구조중에는 무결성을 보장하는 자료구조 또한 존재
- **Lock**을 통해 무결성을 보장할 수 있다.

<br>

#### lock 객체 메서드

- `threading.Lock()`: lock 객체 생성
- `.acquire(True)`: 스레드에서 lock을 잡는 요청
  - 인자에 False를 전달하면 lock을 잡을 때 까지 blocking 되지 않고 다음 로직 수행.
- `.release()`: lock을 헤제

<br>

#### 2개의 스레드가 lock, release를 반복하는 예시

```python
import time
import logging
import threading

logging.basicConfig(level=logging.DEBUG, format="(%(threadName)s) %(message)s")

def blocking_lock(lock):
    logging.debug("Start blocking lock")

    while True:
        time.sleep(1)
        lock.acquire()

        try:
            logging.debug("Grab it")
            time.sleep(0.5)
        
        finally:
            logging.debug("Release")
            lock.release()


def nonblocking_lock(lock):
    logging.debug("Start nonblocking lock")

    attempt, grab = 0, 0

    while grab < 3:
        time.sleep(1)
        logging.debug("Attempt")
        success = lock.acquire(False)

        try:
            attempt += 1
            
            if success:
                logging.debug("Grap it")
                grab += 1
        
        finally:
            if success:
                logging.debug("Release")
                lock.release()

    logging.debug("Attempt: %s, grab: %s" % (attempt, grab))


def main():
    lock = threading.Lock()

    blocking = threading.Thread(target=blocking_lock, name="blocking", args=(lock,))
    blocking.setDaemon(True)
    blocking.start()

    nonblocking = threading.Thread(target=nonblocking_lock, name="nonblocking", args=(lock,))
    nonblocking.start()


if __name__ == "__main__":
    main()


```

- 1개의 lock을 설정
- blocking_lock 스레드는 1초 쉬고, **lock을 잡을 때 까지 대기**하는 작업을 반복
  - lock을 0.5초 동안 잡고 release
- nonblocking_lock은 lock을 잡으려는 시도를 반복
  - attempt와 grab 변수는 lock을 잡으려는 시도, 실제 잡은 횟수를 의미
- nonblocking_lock이 3번의 lock을 쥐면, 프로그램을 종료

<br>

> 결과

![](https://user-images.githubusercontent.com/19590371/67850116-1120ec00-fb4b-11e9-8031-0443864881cf.PNG)

- nonblocking_lock이 5번의 시도 중 3번의 lock을 획득함을 볼 수 있다.

<br>

> nonblocking_lock에서 aquire에 False 인자를 전달하지 않으면

![](https://user-images.githubusercontent.com/19590371/67850118-12521900-fb4b-11e9-9c8d-4cf0912b9de5.PNG)

- nonblocking_lock이 lock을 잡을 때 까지 대기하게 되므로 시도 마다 lock을 획득할 수 있다.

<br>