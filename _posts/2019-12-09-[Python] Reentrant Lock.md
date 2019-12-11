---
title: "Reentrant Lock"
tags: ["Python", "Thread"]
---



## Thread Reentrant Lock

- 스레드에서 락을 잡은 상태에서 다른 클래스나 함수를 호출할 때, 그 클래스나 함수에서 다시 락을 잡아야 하는 경우가 발생 => 교착상태 발생
- 재진입이 가능한 RLock(Re-entrant Lock) 사용



##### 예시 코드

```python
import time
import logging
import threading

logging.basicConfig(level=logging.DEBUG, format="(%(threadName)s) %(message)s")

RESOURCE = 0

def set_zero(lock, end=False):
    logging.debug("Start set zero")
    
    while True:
        with lock:
            logging.debug("Grab Lock, RESOURCE to 0.")
            RESOURCE = 0
            time.sleep(0.5)
        
        time.sleep(1)

        if end:
            break

def set_one(lock, end=False):
    logging.debug("Start set one")
    
    while True:
        with lock:
            logging.debug("Grab Lock, RESOURCE to 1.")
            RESOURCE = 1
            time.sleep(0.5)
        
        time.sleep(1)

        if end:
            break


def set_reverse(lock):
    logging.debug("Start reverse")

    with lock:
        logging.debug("Grab Lock, reverse")

        if RESOURCE == 1:
            set_zero(lock, True)
        else:
            set_one(lock, True)

    logging.debug("Reversed")

def main():
    lock = threading.RLock()

    zero = threading.Thread(target=set_zero, name="zero", args=(lock,))
    zero.setDaemon(True)
    zero.start()

    one = threading.Thread(target=set_one, name="one", args=(lock,))
    one.setDaemon(True)
    one.start()

    time.sleep(6)

    reverse = threading.Thread(target=set_reverse, name="reverse", args=(lock,))
    reverse.start()

if __name__ == '__main__':
    main()
    
```

- `set_one`과 `set_zero`에서 각각 lock을 잡으며, `RESOURCE` 를 1과 0으로 변경하는 작업
- `reverse` 스레드에서는 `set_one`과 `set_zero` 를 호출
- `reverse` 스레드에서 `set_one`과 `set_zero`를 호출하면 각 함수에서 lock을 얻기 위해 무한정 기다릴 수 있다.
- 이를 방지하기 위해 re-entrant lock을 사용하여 `reverse`에서 잡은 락을 `set_zero`나 `set_one`에서 잡을 수 있도록 한다.



##### 실행 결과

<img width="265" alt="" src="https://user-images.githubusercontent.com/19590371/70446292-34db3880-1ae0-11ea-95e4-f1bdea2b30f0.png">



##### RLock을 사용하지 않을 경우

<img width="234" alt="스크린샷 2019-12-10 오후 11 04 48" src="https://user-images.githubusercontent.com/19590371/70536125-87305e00-1ba1-11ea-9fd2-082547eb1957.png">

- `set_reverse` 내부의 `set_one` 을 실행하면서 lock을 잡지 못하기 때문에 교착상태 발생