---
title: Python Flask
tags: ["Python", "Flask"]
---



# Python Flask

##### 2019. 01. 08 (Tue)

<br>

<br>

## I. Flask

#### c9 개념

port : 요청이 들어오는 문

- 서비스 종류에 따라 다름(file, http, ssh, mail...)

c9은 8080포트를 받아 연결을 실행

<br>

<br>

### (1) 작동 방식

함수에서 return을 통해 html 파일을 건네줌

python 깔려있는 어디서든지 실행 가능

app = Flask(\__name__, template_folder='views')
- \__name__ : 파일이 실행되는 컨텍스트
  - app에 Flask의 실행 이름 연결
- template forder 경로 설정

html 파일에서 action으로 바로 특정 페이지로 넘겨줄 수 있음

\<form> : 사용자로부터 받은 특정한 값을 지정한 곳으로 보냄

```html
<form action = "보낼곳">
    <input type="text", name="변수">
</form>
```

<br>

<br>

### (2) request

사용자로 부터 받은 정보를 저장하는 객체

flask에 속함

#### 기능

fullpath : 서버로 요청된 url

remote_addr : 서버로 요청된 ip 주소

url : 서버로 요청된 full url

headers : 헤더정보 출력

args : 변경 불가 dictionary로 (변수명, 변수)를 묶어서 반환

- args.get() : dictionary는 get(key)로도 접근 가능

<br>

<br>

### (3) BeautifulSoup

bs4를 import

웹 페이지를 분석해서 정보 추출

select_one() : 태그명을 따라 웹페이 원소 추출

<br>

<br>

### (4) POST

python에서 requests.post()로 메세지를 전송

requests.post(url, headers(dictionary), files(dictionary))

<br>

<br>

## II. Bootstrap

반응형, 모바일 지향용 front-end template

- 반응형 : 디바이스의 크기에 따라 웹페이지가 반응하여 크기를 조절

jumbotron : 웹페이지 메인의 가장 큰 화면. 전광판

front-end는 bootstrap을 잘 써서 꾸미자



