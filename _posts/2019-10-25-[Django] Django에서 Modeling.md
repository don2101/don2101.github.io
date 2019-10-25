---
title: "Django에서 여러 가지 Modeling"
tags: ["Django"]
---



## 테이블 간의 modeling

<hr>

- 두 테이블 내 레코드 간의 관계로 modeling

<br>

### 1. 1 : 1 관계

- 테이블 2개가 있을 때, 각각 테이블의 **레코드와 레코드가 1 : 1**
  - 예시: 부부의 관계, 주민번호
- 어느 한 쪽에서 봐도 **결정적**. 등가 관계

<br>

### 2. 1 : N 관계

- 하나의 레코드가 여러개의 레코드와 연결
- 1쪽의 레코드가에서는 **n개의 레코드를 소유**, n개 입장에서 하나의 레코드는 **반드시 1에 소속돼야 함**
- **N쪽**에서 보면 **결정적**
  - 예시: 학급 - 학생
    - 학생을 지칭하면 학급을 결정 가능. 학급을 지칭해도 학생 결정 불가능

<br>

### 3. M : N 관계

- M측에서 **하나의 레코드**가 N측의 **여러개 레코드와** 연결
- 또, N측에서 **하나의 레코드**가 M측의 **여러개 레코드와** 연결
- 예시: 수강신청

<br>

### 4. 관계 없음

<br>

<br>

## django에서 modeling

<hr>

### 1. 1 : N (댓글달기)

- **1 : N** 관계
- 무식한 방법: 댓글 갯수 만큼 글에 **column**을 만든다
  - 부적절한 방법...
- comments라는 column을 만들어 string으로 붙임
  - 이거도 부적절...
- comments라는 **table**을 하나 더 만들어 댓글을 관리?
  - 댓글은 게시글과 **특정한 관계**를 갖고 있다!

<br>

#### 테이블 연결 방법

- 1쪽에서 N의 정보를 갖는다: **여러개의 값**이 들어가 비효율적.
- N쪽에서 1의 정보를 갖는다: **N은 반드시 하나의 개체 속해야** 되기 때문에 정보를 하나만 저장해도 된다.
  - **댓글이 게시글의 정보를 갖는다**

<br>

#### 레코드와 레코드를 연결

- 레코드의 특정한 값으로 연결
- N측에서 **1측의 특정한 값을 소유**
- 유일한 값인 primary key로 연결
- **foreign key**: 외부에서 1측의 레코드를 조회하는 key

<br>

#### django에서 구현

- `ForeignKey()` 필드로 구현
- `on_delete`: 소속되고 있는 테이블이 삭제될 시 행동
  - `CASCADE`: **같이 삭제**. 일반적
  - `PROTECT`: 지우지 않고 보존
  - `SET_NULL`: NULL값으로 설정
  - `DO_NOTHING`: 아무행동도 하지 않는다

```python
article = models.ForeignKey(<속하는 테이블 명>, on_delete=models.CASCADE, related_name="comments")
```

- 실제 구현시 foreign key에 _id붙여서 column 생성
- **related_name**: 1측에서 **N측을 조회**할 수 있는 이름

<br>

#### 값을 집어넣기

- `id`로 primary key 삽입 가능
- django는 소속될 **객체 자체를 삽입**해도 가능
  - 객체를 조회하면 소속되고 있는 **객체도 조회 가능**

<br>

##### 코드 예시

```python
# foreign_key로 연결

comment = Comment(content="cmt", article_id=1)
# 객체 차체로 연결

comment = Comment(content="cmt", article=article)
# 연결된 article 조회

Comment.objects.first().article
# == Article.objects.get(pk=Comment.objects.first().article_id)
```

<br>

#### 조회 방법

- 양쪽 모두에서 조회 가능

> 1 입장에서 N을 조회

```python
Article.objects.get(article_id=1).comment_set.all()
```

<br>

> N입장에서 1을 조회

```python
comment = Comment.objects.filter(article_id=pageNum).all()
```

<br>

### 2. M:N(수강신청)

- 기본적으로 1:N을 사용하여 해결
- **2개 테이블의 PK**를 데이터로 갖는 또 다른 테이블을 생성
  - 결국에 1:N 관계 2개를 사용하여 푼 것이 됨

<br>

#### Meta class

- 메타 데이터: 모델에 대한 데이터. 데이터에 대한 데이터

<br>

#### ManyToManyField()

- 그래도 테이블 따로 안만들고 직접 M:N 관계를 만들고 싶다
- M:N 관계를 맺어주는 속성
- `ManyToManyField("모델 클래스 이름")`
- `on_delete` 필수 아님
- 방향도 정할 필요가 없다
  - 양쪽 모두에서 사용가능
- 그러나 순서가 중요
  - python은 위에서 아래로 읽기 때문에 

<br>

##### 사용 예시

```python
class Client(models.Model):
    name = models.CharField(max_length=30)
    
    
class Resort(models.Model):
    name = models.CharField(max_length=30)
    clients = models.ManyToManyField(Client)
```

<br>

> migration 결과

```sql
python manage.py sqlmigrate sugangs 0002
BEGIN;
--
-- Create model Client
--
CREATE TABLE "sugangs_client" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "name" varchar(30) NOT NULL);
--
-- Create model Resort
--
CREATE TABLE "sugangs_resort" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "name" varchar(30) NOT NULL);

--------------------------------------------------------------------------------------
CREATE TABLE "sugangs_resort_clients" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "resort_id" integer NOT NULL REFERENCES "sugangs_resort" ("id") DEFERRABLE INITIALLY DEFERRED, "client_id" integer NOT NULL REFERENCES "sugangs_client" ("id") DEFERRABLE INITIALLY DEFERRED);
--------------------------------------------------------------------------------------
...
```

- django가 **sugangs_resort_clients**라는 테이블을 자동으로 생성
- 이후 django를 통해 client, resort 테이블을 연결하여 사용 가능

<br>

> resort 기준으로 client 조회

```python
resort = Resort.objects.get(pk=3)

resort.clients.all()
```

<br>

> resort 기준으로 client 추가

```python
resort.clients.add(Client.objects.first())
```

<br>

> resort 기준으로 client 삭제

```python
resort.clients.remove(Client.objects.first())
```

- 함수에 객체를 대입하여 사용

- 중복되는 데이터는 django 알아서 filtering

<br>

#### Client 기준에서?

- **resort_set**으로 조회 가능
- resort기준에서 처럼 바로 조회하고 싶다면...

```python
clients = models.ManyToManyField(Client, related_name='resorts')
```

- Resort 클래스에서 **related_name** 필드 추가

<br>

### 3. 1 : 1 (User Profile)

#### 구현 방법

- django에서 기본적으로 구현되어 있는 `User` 모델을 직접 수정하지 않고 컬럼을 추가
- `User` 테이블과 1:1 관계를 갖는 테이블을 만들어 연결
- `OneToOneField()`: 1:1 관계 테이블 직접 참조

<br>

##### 예시

```python
class UserProfile(models.Model):
    user = models.OneToOneField(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    nickname = models.CharField(max_length=45, null=False, unique=True)
    age = models.IntegerField()
```

- django에서 기본적으로 정의된 `User` 모델과 연결할 `UserProfile` 모델

<br>

#### 조회

> User에서 UserProfile 조회

```python
user = User.Objects.get(pk=1)
user.userprofile.age
```

<br>

> UserProfile에서 User 조회

```python
user_profile = UserProfile.objects.get(pk=1)
user_profile.user.id
```

