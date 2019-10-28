---
title: "Django Serializer"
tags: ["Django", "DRF"]
---



## Django Rest Framework

### 1. Client - Server 구조

- 기존에는 django에서 server와 frontend 기능을 모두를 수행하는 구조를 갖고 있었다.

- django하나에서 데이터 조작과 html render 둘다 수행.



#### REST API 서버

- **django**를 **Rest-API 서버**로 구성한다.

- frontend 서버에서 들어온 **요청에 따라 데이터를 조작**하고, 결과를 **Json형식**으로 frontend 서버에에 넘겨준다.

- 서버는 frontend의 요청에 따라서 **정보를 제공**하는 역할만 수행



#### Frontend 서버

- frontend 서버는 ajax통신을 통해 API 서버로 데이터를 요청.

- API서버에서 Json으로 넘어온 **데이터를 파싱**하고, 사용자에게 보여질 데이터를 **rendering**한다.

- frontend 서버가 **요청**하고, API 서버가 **응답**하는 **client - server 구조**에 따라 서비스를 구성



### 2. Django API 서버 구축

#### django rest framework

- django로 api서버를 구축하는 것을 도와주는 패키지



##### 설치

```bash
pip install djangorestframework
```



##### INSTALLED_APPS에 추가

```python
INSTALLED_APPS = [
	# ...
    'rest_framework',
]
```



### 3. Serializer

- API 서버와 frontend서버 통신할 때, 보통 `Json`형식으로 통신하게 된다.

- `json`으로 전달된 데이터는 서버의 언어(python)로 인식할 수 있는 자료형으로 변환되어야 한다.
- 마찬가지로 서버에서 생성한 데이터를 frontend에서 읽기 위해 `json`으로 파싱해야 한다.
- **Serializer**는 `QuerySet`이나 `Json`등 복잡한 자료형을 Python 기본 데이터 타입(`dictionary`)으로 변환해주는 기능.

- 역변환 또한 제공하며, python 기본 데이터(`dictionary`)를 지정한 `QuerySet`이나 `Json`으로 변경할 수 있다.

`.data`

- dictionary로 파싱된 데이터에 접근

`.is_valid()`

- 유효성 검사

`.errors`

- 유효성 검사중 발생한 에러 반환

`.save()`

- 객체를 Model로 변환하여 저장





## django에서 serializer 사용

### 1. Model과 연결된 Serializer 사용

- serializer는 Model의 **어떤 컬럼**을 **어떻게 직렬화하느냐**를 정의한다
- `Meta` 클래스에서 연결할 Model을 정의하고, 표현할 column을 정의



#### Serializer 정의

##### Model

```python
from django.db import models

# Create your models here.

class Memo(models.Model):
    title = models.TextFile()
    content = models.TextField()
    
    def __str__(self):
        return f'{self.content}'
```



##### ModelSerializer

```python
# serializers import

from rest_framework import serializers
from .models import Memo


# 선택된 model에 직렬화 기능 추가

class MemoSerializer(serializers.ModelSerializer):
    # 필드에 대한 옵션을 직접 설정 가능
    
    content = serializers.CharField()
    
    class Meta:
        model = Memo
        # fileds에 직렬화 할 컬럼을 정의한다.
        
        fields = ['content', ]
        # "__all__"을 정의하면 모든 컬럼에 대해 직렬화
        
        # fields = "__all__"
```

- Model에 Serialization 기능을 쉽게 추가

- `ModelForm`과 유사하게 작동하며, 선택된 필드를 참조한다.

- 유효성 검사 또한 가능하다.
- 필드에 대한 옵션을 직접 설정 가능하며, 설정하지 않으면 model에서 정의한 옵션을 따른다



#### 데이터 조회

- Serializer를 생성하며, 정보를 조회할 **인스턴스**를 인자로 넘겨준다
- 해당 인스턴스의 **serializer에서 정의된 field**에 대해서 조회



##### 예시

```python
def get_memo(request, memo_id):
    # 조회할 memo 인스턴스 생성

    memo = Memo.objects.get(pk=memo_id)
    # serializer를 통해 data를 직렬화
    
    # MemoSerializer에서 content에 대해서만 fields를 정의했으므로 content만 직렬화
    
    serializer = MemoSerializer(memo)
    
    # 직렬화 한 데이터를 반환
    
    return Response(data=serializer.data)
```



##### write_only 옵션

```python
class MemoSerializer(serializers.ModelSerializer):
    # 쓰기 전용 옵션 설정

   	content = serializers.CharField(write_only=True)
    
    class Meta:
        model = Memo
        
        fields = ['content', ]
```

- serializer에서 field를 정의할 때,  `write_only` 옵션을 추가하여 속성을 쓰기전용으로 생성 가능
- 쓰기전용으로 정의한 field는 **조회할 때 직렬화 되지 않는다**.



##### many 옵션

```python
memos = Memo.objects.all()

serializer = MemoSerializer(memos, many=True)
```

- 다수의 인스턴스를 직렬화 할 때 사용
- list로 데이터를 직렬화



#### 데이터 저장

- serializer는 직렬화에 대한 기능만 수행, 저장은 Model에서 수행할 문제
- serializer는 데이터를 **어떤 데이터를 어떻게 넘결지만** 정의한다.



##### 예시

```python
def post_memo(request):
    # Serializer를 생성하며 data 인자로 request.data에 있는 값을 넘겨준다

	serializer = MemoSerializer(data=request.data)
    # 유효성 검사
    
    if serializer.is_valid():
        # 모델에 저장
        
        serializer.save()
```

- Serializer의 data인자로 `request.data`를 넘기면, json의 `key ` 값과 serializer의 `field` 값을 비교하여, **이름이 일치하면** 데이터를 삽입
- `.is_valid`는 유효성 검사를 실시하며, model이나 serializer에서 정의한 조건을 수행
- `.save()`는 serializer와 연동된 model에 데이터를 저장하며, 해당 model의 **인스턴스를 반환**



##### partial 옵션

```python
serializer = MemoSerializer(data=request.data, partial=True)
```

- `request.data`의 데이터 중 일부만을 직렬화 하며 삽입할 때 사용
- request.data의 데이터 중 `key` 값이  serializer의 `field`와 일치하는 것만 삽입





> dictionary를 Model로 변환

`data` 인자로 데이터를 받는다.

```python
serializer = MemoSerializer(data=request.data)
```



> Model을 dictionary로 변환

`many = True` 옵션을 사용하여 다수의 데이터를 받을 수 있음

```python
memos = Memo.objects.all()
serializer = MemoSerializer(memos, many=True)
```





### (2) API 기능 구현

frontend 서버가 요청한 기능을 수행하고 그에 따른 응답을 구현

class 내부에서 메서드를 사용한 구현과 view단에서 함수를 사용한 구현이 있으며, 여기서는 함수를 사용한 방법으로 구현한다.



#### 1. request, response

기본적으로 클래스방식이든, view를 사용한 방식이든 클라이언트 - 서버간 통신하는 방법은 http로 같다.

하지만, 요청과 응답에 대해서 django가 정의한 `HttpRequest`, `HttpResponse`가 아닌 `request`, `response` 객체를 사용한다.

`request`와 `response`는 django rest framework에서 제공하는 객체로, Json - 기본 데이터간 유연한 파싱이 가능하다.

Rest API 서버는 `request`, `response`를 사용하여 frontend 서버의 요청에 응답한다.



> **data**

- python의 dictionary로 파싱된 request body의 데이터를 반환한다. django의 `request.POST`, `request.FILES`와 유사한 기능
- file, non-file 데이터 등, 모든 파싱된 content에 대해 작동한다.

> **user**

- djagno의 `request.user` 처럼 현재 접속한 사용자를 반환한다.



#### 2. @api_view

view의 함수가 Rest API의 기능을 수행하도록 감싸주는 decorator

해당 decorator를 함수 앞에 선언하는 것으로 함수가 **Rest API의 기능을 수행**한다.

인자로 rest framework의 **request 객체**를 받으며, **response 객체**를 반환한다.

> 데코레이터에 허용하는 메서드 추가

```python
@api_View(['POST', 'GET'])
```



##### 구현 예시 코드

```python
# Response import
from rest_framework.response import Response
# api_view import
from rest_framework.decorators import api_view
from .models import Memo
from .serializers import MemoSerializer


# api_view 기능 적용
@api_view(['GET', 'POST'])
def memo(request):
    if request.method == 'POST':
        # request로 받은 데이터를 Memo Model로 역직렬화
        # data 인자로 request.data를 넘김
        serializer = MemoSerializer(data=request.data)
        
        # raise_exception: exception 처리 없이 넘어간다
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            
            # dictionary로 파싱된 데이터 반환
            return Response(serializer.data)
       	return Response(serializer.error)
    else:
        memos = Memo.objects.all()
        serializer = MemoSerializer(memos, many=True)

        return Response(serializer.data)
        
```





### (3) CORS

이렇게 기능을 구현하고, frontend 서버에서 자료를 요청하더라도 작동하지 않는다.

에러 문구를 확인하면

```
blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

와 같은 에러가 발생한다. CORS 정책에 의해 요청이 막혔다는 내용이다.



#### CORS?

CORS: cross origin resourse sharing으로 요청에 따라 데이터를 주고받는 것을 의미한다.

모든 요청에대해 resource를 제공하게 되면, 문제가 발생할 수 있으므로, 보안상의 이유로 resource에 대한 직접 접근은 지양된다.

따라서, API 서버에서 다른 origin(domain)에게 resource 접근 권한을 허가해주어야 자료에 접근할 수 있다.

django의 middleware에 이를 추가하여 접근을 허가할수 있다.



#### middleware

http 요청 / 응답 처리 중간에서 작동하는 시스템. django 전반적인 부분에서 동작한다.

클라이언트에서 보낸 http 요청을 확인하고, 요청에 대한 작업을 확인하고 응답하게 하는 장치



##### 미들웨어의 순서

- http **request**가 들어오면 **위에서 아래**로 미들웨어를 적용한다.
- http **response**를 반환하면 **아래에서 위**로 미들웨어를 적용한다.

