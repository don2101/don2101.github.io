---
title: "Django에서 queryset 사용하기"
tags: ["Django"]
---



## Queryset의 특징

<hr>

#### queryset

- django에서 ORM을 사용하기 위한 DB의 레코드

<br>

### 1. Queryset은 마지막 까지 지연(lazy)된다

- 필터링을 하는 경우 `.filter()` 를 사용
- `.filter()` 를 사용한다 하더라도, django는 DB에 아무 쿼리도 전달하지 않는다.
  - 실제로 데이터에 접근하는 로직에만 작동
  - 이는 DB에 접근하는 횟수를 줄여 웹 앱이 느려지게 하는 것을 방지한다
- 최종적으로 queryset을 순회(iterate)하는 경우에만 레코드를 가져온다

<br>

##### filter & iteration

```python
post_set = Posts.obejcts.filter(title="branch")

for post in post_set:
    print(post.title)
```

- 글 제목이 title인 모든 글을 나타낸다.
- 최종적으로 iteration 하는 경우에만 query 실행

<br>

### 2. queryset은 캐시된다

#### evaluation

- queryset을 순회하는 시점에 쿼리셋에 해당하는 DB의 레코드를 가져온다(`fetch`)
- 또한, 이는 모두 django model로 변환되며 이를 평가(`evaluation`)이라 한다.

<br>

#### cashing

- 평가된 모델은 queryset 내장 캐시에 캐시된다.
- 또다시 queryset을 순회해도 같은 query를 DB에 전달하지 않는다.

<br>

> 두번의 순회를 하더라도 한번만 query를 전달

```python
for post in post_set:
    print(post.title)
    
for post in post_set:
    print(post.title)
```

<br>

### 3. If 문에서는 queryset evaluation 발생

- queryset에 레코드가 있는지 검사한 후 있을 때만 순회하도록 할 수 있다.

<br>

##### 사용 예시

```python
post_set = Posts.objects.filter(title="branch")

# if문에서 queryset evaluation 실행

if post_set:
  # 순회시에는 캐시된 queryset 사용
  
  	for post in post_set:
    		print(post.title)
```

<br>

### 4. 결과 전체가 필요하지 않은 경우 queryset 캐시는 비효율적

```python
post_set = Posts.objects.filter(title="branch")

# queryset eavalution 발생

if post_set:
  	print("For only one query, whole size of queryset is fetched")
```

- if문이 queryset evaluation을 발생시키므로 하나의 결과를 위해 전체 query를 가져오게 되어 비효율적이다

<br>

#### exist()

- 최소한 하나의 레코드가 존재하는지 판단

```python
post_set = Posts.objects.filter(title="branch")

if post_set.exists():
  	print("Exist method is useful for checking there is queryset has a record")
```

- `.exists()` 메소드는 최소한 하나의 레코드가 존재하는지 판별한다.

<br>

### 5. queryset이 엄청 큰 경우 queryset 캐시는 문제가 된다

- 수많은 레코드를 다루는 경우 한번에 메모리에 올리는 것은 비효율적
- 거대한 쿼리가 서버의 프로세스에 lock을 걸어 app이 죽을 수도 있다.

<br>

#### iterator()

- queryset 캐시를 방지하며 전체 레코드를 순회해야 할 때 사용
- 데이터를 작은 덩어리로 쪼개서 가져오며, 사용한 레코드는 메모리에서 지운다.

```python
post_set = Posts.objects.all()

for post in post_set.iterator():
  	print(post.title())
```

- 전체 레코드의 일부만 가져오브로 메모리를 절약할 수 있다.
- 그러나 같은 queryset을 순회하면 다시 evaluation이 발생하므로 조심해야 한다.

<br>

### 6. queryset이 엄청 큰 경우 if문도 문제가 된다

- queryset 캐시와 if/for문을 사용하여 상황에 따라 queryset을 순회하는 것을 정할 수 있다.
- 그러나, queryset이 큰 경우 queryset 캐시는 고려할 수 없으므로 다른 방법을 사용

<br>

#### exists()와 iterator()를 함께 사용하는 방법

```python
post_set = Posts.objects.all()

# exists()메서드로 레코드가 존재하는지 검사

if post_set.exists():
  	# 다음 query로 레코드를 조금씩 가져온다
  
  	for post in post_set.exists():
      	print(post.title)
```

<br>

#### 고급 순회 메서드

- 순회 진행을 정하기 전에 iterator() 메서드의 첫번째 레코드를 감지(peek)

```python
post_set = Posts.objects.all()

# 이 때부터 첫 번째 query로 레코드를 가져오기 시작한다

post_iterator = post_set.iterator()

# 첫 번째 레코드를 감지(peek)

try:
  	first_post = next(post_iterator)
except StopIteration:
  	# 레코드가 없으면 아무일도 하지 않는다.
		pass
else:
  	# 레코드가 하나라도 존재하면, 모든 레코드를 순회
    from itertools import chain
    for post in chain([first_post], post_set):
      	print(post.mass)
```











