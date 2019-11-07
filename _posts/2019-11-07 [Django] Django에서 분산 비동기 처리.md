---
title: "Celery를 사용한 Django에서 분산 비동기 처리"
tags: ["Django", "Celery"]
---



## Celery

<hr>

- 웹 서비스에서 응답을 받기 까지 오래 걸리는 작업은 사용자가 기다리기 힘들다
- 이렇게 비용이 무거운 로직은 **비동기**로 처리하여, 사용자가 응답을 받은 후 결과를 받게 할 수 있다.
- Celery는 django에서 비동기 처리를 도와주는 프레임워크(worker)



### 1. 설치(Ubuntu)

- Celery 설치

```bash
pip install celery
```



### 2. 브로커 설정

#### 브로커

- 웹 서버에서 작업을 브로커에게 전달하고, celery 워커가 작업을 받아서 처리
- 