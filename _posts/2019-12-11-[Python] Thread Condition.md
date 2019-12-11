## Thread Condition

- Event와 Lock을 섞은 기능
- Condition을 잡으면 모든 스레드가 lock을 잡은 것처럼 멈춘다
- `notify` 를 실행하면 다시 동작



```python
import time
import logging
import threading


logging.basicConfig(level=logging.DEBUG, format="(%(threadName)s) %(message)s")


def receiver(condition):
    logging.debug("Start reciever")

    with condition:
        logging.debug("Waiting")
        condition.wait()
        time.sleep(1)
        logging.debug("End")


def sender(condition):
    logging.debug("Start sender")

    with condition:
        logging.debug("Send notify")
        condition.notifyAll()
        logging.debug("End")


def main():
    condition = threading.Condition()

    for i in range(5):
        t = threading.Thread(target=receiver, name="receiver %s" %i, args=(condition, ))

        t.start()

    send = threading.Thread(target=sender, name="sender %s" %i, args=(condition, ))

    time.sleep(1)
    with condition:
        condition.notify(1)

    time.sleep(3)
    send.start()


if __name__ == "__main__":
    main()
```

- 5개의 스레드가 `notify` 를 기다리는 상황
- 메인 스레드에서 하나의 스레드에 `notify` 를 보냄
- 이후 모든 스레드에게 `nofity` 를 보내는 상황



#### condition

- lock과 비슷하게 사용되며, 사용하는 스레드가 공유
- `notify` 가 올때까지 기다리며, `notify` 를 받으면 그 이후를 실행
- 마지막에 있는 `time.sleep()` 에 10초를 넣으면 10초동안 1, 2, 3, 4번 스레드가 기다린다.

