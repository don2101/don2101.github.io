---
title: "Django의 Model"
TAG: ["Django", "Python"]
---



## Django Model

<hr>

- django에서 데이터를 Database에 저장, 조회, 수정, 삭제를 하는데 사용
- 클라이언트에 제공할 정보를 Class로 조작한다

<br>

### 1. Django ORM

- SQL로 DB를 조작하는 것이 불편하다
- Class에 속성을 정하고 Class를 통해 DB를 조작하면 편할것 같다
- ORM은 framework에서 Class를 통해 DB를 조작하는 방법

<br>

#### Schema 정의

- `model.py`에서 class로 정의
- Class 이름으로 table 생성
- `models.Model`을 상속받아 정의
- `id` 정의하지 않아도 알아서 id column 정의
- `__repr__()`:을 정의하여 편하게 사용 가능
  - migrate하지 않아도 적용가능. 데이터가 아닌 **행동양식만 변경**
- `__str__()`: print에 출력할 내용

<br>

##### 코드 예시

```python
from django.db import models

# DB에 생성될 Table을 정의

class Article(models.Model) :
    # DB 속성을 정의
    
    title = models.TextField()
    content = models.TextField()
    
    def __repr__(self) :
        return f"title: {self.title} content: {self.content}"
        
    # 출력 양식을 정의. 이는 DRF에서 StringRelatedField와 연결된다
    
    def __str__(self) :
        return f"title: {self.title} content: {self.content}"
```

<br>

#### Schema 적용

**make migration file**: class로 생성한 schema를 django orm에 넘기는 과정

```bash
> python manage.py makemigrations
```

- `model.py`에 있는 코드를 기준으로 schema생성
- migrations 폴더 내에 schema 파일 생성
- migration file은 **직접 조작하지 않고**, `model.py`를 통해서 **django가 조작**하게끔 한다.

<br>

**migrate**: django orm이 schema를 DB에 적용하는 과정 

```bash
> python manage.py migrate
# table 생성시 적용한 sql문 출력

> python manage.py sqlmigrate <table 이름> <migration 파일>
```

- schema를 DB에 적용 

<br>

<br>

## CRUD 연산

<hr>

### 1. Create

- 객체를 정의하여 데이터 저장

<br>

##### 코드 예시

```python
a = Article(title="happy", content="hacking")
a.save()
```

```python
Article.objects.create(title="hello", content="world!")
```

<br>

### 2. Read

- `QuerySet` 객체로 데이터를 반환

<br>

##### 코드 예시

```python
Article.objects.all()
Article.objects.first().title
>>> 'happy'
Article.objects.first().content
>>> 'hacking'
```

<br>

#### QuerySet?

- List와 유사한 객체
- django orm이 CRUD를 위해 사용하는 데이터 객체
- **.\<속성>**으로 속성 접근
- 모든 리스트 조작이 가능

<br>

#### filter

- where절

<br>

##### 코드 예시

```python
Article.objects.filter(title="happy").all()
Article.objects.filter(title="happy").first()
```

<br>

#### Count

##### 코드 예시

```python
Article.objects.filter(title="happy").count()
```

<br>

#### get

- 조건을 기준으로 하나의 레코드 출력

<br>

##### 코드 예시

```python
Article.objects.get(id=1)
# pk = primary key

Article.objects.get(pk=1)
Article.objects.get(title="happy")
```

<br>

#### order_by

##### 코드 예시

```python
Article.objects.order_by('id').all()
```

<br>

#### limit, offset

- **List 조작**을 통해 구현

```python
Article.objects.all()[5:10]

```

<br>

### 3. update

- 객체를 가져와 속성을 수정하고 직접 저장

<br>

##### 코드 예시

```python
a = Article.objects.get(id=1)
a.content = "somthing"
a.save()
```

<br>

### 4. Delete

##### 코드 예시

```python
a = Article.objects.get(id=1)
a.delete()
```

<br>

<br>

## Django 명령어

<hr>

### 1. shell

```bash
python manage.py shell
```

- 프로젝트 내에서 shell 실행

<br>

### 2. admin.py

- Model에 대한 관리를 설정
- 관리자의 Model 관리 기능
- 우리가 **정의하지 않은 url을 포함**

<br>

> Model 추가

```python
# admin.py

admin.site.register(Job)
```

- Model을 추가할 경우 admin 페이지에서 Model을 관리할 수 있으며, 편하게 볼 수 있다.

<br>

> super user 추가

```shell
python manage.py createsuperuser
```

<br>

> migration file 생성

- model.py에 있는 코드를 기준으로 schema생성

```shell
python manage.py makemigrations
```

<br>

> migrate

- migration file을 기준으로 DB에 table 생성

```shell
python manage.py migrate
```

<br>

> table 생성 sql문 출력

- table 생성시 사용한 sql문 출력

```shell
python manage.py sqlmigrate <table 이름> <migration 파일>
```

<br>

<br>

## 기타 내용

<hr>

### 1. base html 설정

- {% block \<변수명> %} {% blockend %}: base.html에서 빈 구멍 생성
- {% extends base.html %}로 상속 후, {% block \<변수명> %} {% blockend %}안에 내용 작성

<br>

### 2. Css 적용

```html
{% load static %}

<link rel="stylesheet" href="{% static 'css/style.css' %}">
```

- **static 폴더 불러온 후**
- static 파일(**이미 정의됨**) 내부의 css폴더의 style.css 적용
- django template language 사용해서 적용

<br>

