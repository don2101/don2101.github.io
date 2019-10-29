---
title: "Thread의 Event"
tags: ["Python", "Thread"]
---



## Thread Event

<hr>

- 파이선의 threading 모듈에서 **스레드간 통신**을 위해 이벤트를 사용
- 이벤트 설정, 초기화, 기다리기 등의 동작을 제공
- **특정 조건**에 따라 스레드를 동작시킬 때 유용
- 스레드의 **인자로 넘겨** 이벤트를 확인한다

<br>

### 메서드

- `threading.Event()`: 이벤트 객체 생성
- `.isSet()`: 이벤트가 설정 되었는지 확인
- `.wait(num)`: num 초 동안 대기

<br>

##### 2개의 Event를 다루는 예시 코드

```python
import time
import logging
import threading

logging.basicConfig(level=logging.DEBUG, format="(%(threadName)s) %(message)s")

def first_wait(e1, e2):
    while not e1.isSet():
        event = e1.wait(1)
        logging.debug("Event status: (%s)", event)
        
        if event:
            logging.debug("e1 is set.")
            time.sleep(3)
            logging.debug("Set e2")
            e2.set()
            

def second_wait(e2):
    while not e2.isSet():
        event = e2.wait(1)
        logging.debug("Event status: (%s)", event)

        if event:
            logging.debug("e2 is set.")
            

def main():
    # Event 객체 생성
    
    e1 = threading.Event()
    e2 = threading.Event()
		
    # 두 이벤트를 시작한다
    
    t1 = threading.Thread(name="first", target=first_wait, args=(e1, e2))
    t1.start()

    t2 = threading.Thread(name="second", target=second_wait, args=(e2, ))
    t2.start()

    logging.debug("Wait...")
    time.sleep(5)
    logging.debug("Set e1")
    e1.set()
    time.sleep(5)
    logging.debug("Exit")
    

if __name__ == '__main__':
    main()
```

- 프로그램을 **2개의 흐름**으로 나눈다
- 스레드를 start 하면 이벤트가 시작된다
- 이벤트 e1은 5초동안 event가 set 되었는지 확인.
  - e1이 set되면 종료
- 이벤트 e2는 e1이 완료된 후 3초동안 set 되었는지 확인

<br>

> 결과

![](https://user-images.githubusercontent.com/19590371/67776646-f21b4f00-faa3-11e9-9679-c6242e276fd3.png)

<br>