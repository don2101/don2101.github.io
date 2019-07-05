---
title: "[Message Framework]"
tags: ["Django", "messages"]
---





# message framework

##### 2019. 07. 05 (Fri)

django에서 **1회성** messages를 저장하고, 출력을 도와주는 framework

messages는 1회만 노출되고, 새로고침하면 사라진다.

messages는 객체 리스트이므로 iteration이 가능하다.

> ex) 로그인(로그아웃) 되었습니다.

<br>

<br>

## message 사용 설정

<hr>

### 1. message 등록 설정

django는 이미 message 사용을 위한 설정을 마련해 놓았다.

- INSTALLED_APPS
- MIDDLEWARE
- TEMPLATES

등에 message 사용을 위한 설정이 되어있다.

<br>

<br>

### 2. message storage back-end 설정

django는 message를 임시로 저장하기 위한 3개의 저장소 클래스를 제공한다.

저장소 클래스는 `django.contrib.messages`에 저장되어 있다.

<br>

#### *class* storage.session.SessionStorage

메세지를 request객체의 session에 저장

<br>

#### *class* storage.cookie.CookieStorage

메세지를 cookie에 담아서 저장. cookie size가 2048byte가 넘어가면 오래된 message는 자동으로 삭제

<br>

#### *class* storage.fallback.FallbackStorage

메세지를 먼저 cookie 저장하고, 더이상 cookie에 저장할 수 없는 경우 session에 저장

<br>

message storage는 `settings.py`에 다음과 같이 선언하여 사용한다.

```python
MESSAGE_STORAGE = 'django.contrib.messages.storage.cookie.CookieStorage'
```

<br>

<br>

<br>

## message객체 사용

<hr>

### 1. message level

일반적인 logging 모듈과 같이 상황에 따른 선언 level이 존재한다.

level마다 출력이 되는 방식은 동일하지만, message를 구분하고 상황에 맞는 level을 사용한다.

<br>

#### level 종류

- DEBUG: 개발과 관련된 message를 출력하는데 사용. 일반 배포 버전에서는 출력되지 않는다.
- INFO: 사용자에게 알리기 위해 출력하는 level
- SUCCESS: 어떤 행위나 작동이 성공했음을 알림
- WARNING: 작동에 오류가 발생할경우 알림
- ERROR: 작동에 오류가 발생하여 실행을 완료하지 못했을 경우 알림

<br>

<br>

### 2. django에서 구현

message를 session에 저장하여 사용하는 방법을 적용

<br>

##### settings.py 에 추가

```python
MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'
```

<br>

##### views.py에서 사용

```python
# messages 객체를 불러온다
from django.contrib import messages

# 로그인 기능
def sign_in(request):
    if request.method == "POST":
        forms = UserLoginForm(request, request.POST)

        if forms.is_valid():
            login_user = forms.get_user()
            login(request, login_user)
            
            # messages객체 생성
            messages.success(request, "로그인 됐습니다")
            
            return redirect('posts:main')
    else:
        forms = UserLoginForm(request)

        return render(request, 'accounts/sign_in.html', {'forms': forms})
```

<br>

##### base.py에서 사용

```html
<!-- messages 리스트가 있다면 iteration으로 하나씩 출력 -->
{% if messages %}
    {% for message in messages %}
        <script>
            alert('{{ message }}');
        </script>
    {% endfor %}
{% endif %}
```

<br>

##### 로그인 결과

![login](https://user-images.githubusercontent.com/19590371/60692367-aa7b2100-9f10-11e9-89d8-84170d81b91f.PNG)

