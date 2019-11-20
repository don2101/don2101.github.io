---
title: "Celery를 사용한 Django에서 분산 비동기 처리"
tags: ["Django", "Celery"]
---



## Celery

<hr>

- 웹 서비스에서 응답을 받기 까지 오래 걸리는 작업은 사용자가 기다리기 힘들다
- 이렇게 비용이 무거운 로직은 **비동기**로 처리하여, 사용자가 응답을 받은 후 결과를 받게 할 수 있다.
- Celery는 django에서 비동기 처리를 도와주는 프레임워크(worker)

<br>

### 1. 설치(Ubuntu)

- Celery 설치

```bash
pip install celery
```

<br>

### 2. 브로커 설정

#### 브로커

- 웹 서버에서 작업을 브로커에게 전달하고, celery 워커가 작업을 받아서 처리
- 브로커는 웹에서 전달한 작업 요청을 **큐**에 담아 보관하고, 워커에 적절하게 분배
- 브로커의 종류는 `rabbitmq`, `redis` 등을 사용할 수 있음



> Celery 작동 구조

![](https://user-images.githubusercontent.com/19590371/68525778-74173d80-0318-11ea-92b2-60c28d599c78.png)

<br>

#### rabbitmq 설치 및 유저 추가

```bash
apt-get install rabbitmq-server # rabbitmq 설치
rabbitmqctl add_user 사용자명 비밀번호 # 유저 추가
# server 실행. -detached: 백그라운드로 실행

rabbitmq-server -detached 
rabbitmqctl stop # rabbitmq 중지

rabbitmqctl start_app # localhost에서 어플리케이션 실행
rabbitmqctl stop_app # 어플리케이션 중지
```

- Rabbitmq에 접근해서 자원을 사용할 유저를 생성

<br>

#### vhost 추가 및 권한 설정

```bash
rabbitmqctl add_vhost /vhost # /vhost라는 vhost 추가
# /vhost와 guest를 연결. guest는 /vhost를 통해 자원에 접근

rabbitmqctl set_permissions -p /vhost guest ".*" ".*" ".*"
```

- rabbitmq의 각 유저가 사용할 가상 호스트를 추가한다
- vhost: rabbitmq의 자원에 접근할 수 있게 하는 인터페이스. 혹은 논리적 그룹.
  - 유저는 vhost를 통해 rabbitmq를 사용한다
- `set_permissions` 명령어를 통해 유저에게 message queue의 자원에 `configure`, `writer`, `read`할 권한을 부여한다.

<br>

### 3. worker 생성

> 프로젝트 구조

```bash
├── celery_testing
├── db.sqlite3
├── manage.py
└── testing
```

<br>

##### workers.py

```python
from celery import Celery

# Celery 객체를 정의. 객체의 이름과 브로커로 사용할 서버 주소를 설정
app = Celery('tasks', broker="amqp://<유저>:<비밀번호>@localhost:5672//<vhost이름>")

@app.task
def sum_number(x, y):
    num = 0

    for i in range(x) :
        for j in range(y):
            num += 1

    return num
```

- 비용이 많이 소모되는 작업 수행시 사용자에게 처리 완료를 알리고, 비동기로 처리
- 비동기로 처리할 작업을 모듈화 

<br>

##### views.py

```python
from django.shortcuts import render
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .workers import sum_number

# Create your views here.
@api_view(["GET"])
def sum_it(request, x, y):
    sum_number.delay(x, y)
        
    return Response(data={"res": "good"})
```

- `.delay()`는 해당 작업을 비동기로 처리함을 의미
  - `.delay()`를 사용하지 않고 실행하면 일반적인 실행 방식으로 실행된다.

<br>

##### settings.py

```python
CELERY_IMPORTS = ["testing.workers", ]
```

- django application내에 비동기 처리 모듈을 import

<br>

### 4. celery 실행

```bash
celery -A 실행app이름 worker --loglevel=info
```

- 실행할 어플리케이션 이름(여기서는 workers)를 celery가 worker로 실행한다
- loglevel을 설정하여 로깅도 가능

<br>

> 결과

![](https://user-images.githubusercontent.com/19590371/68920421-84b43180-07b8-11ea-908a-56281de51983.png)

- url을 통해 10000*10000 연산을 요청할 경우 결과가 바로 return된다.
- 연산은 비동기적으로 실행되며 5.5초 후 연산이 완료됨을 알려준다.

