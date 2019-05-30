---
title : "Chatbot Project 5th"
tags: ["Chatbot", "Request", "Webhook"]
---

# Python Chatbot 5 일차

#### 2018. 12. 21 (Fri)

Chatbot 완성

POST 서비스 요청

네이버 클로바 얼굴 인식 기능 추가

<br>

##### 컴퓨터 공학은 **abstraction** : 요약, 중요한 부분만 추출

하고 싶었던 것을 계속 끌어내보기

<br>

<br>

## I. 서비스 요청 방식

서버 :  요청을 받아 해당하는 서비스를 제공하는 객체

GET, POST 두 방식 모두 데이터를 보내는 방식

<Br>

### (1) GET

데이터를 가져오는 요청 ex) 사진을 주세요

기본적으로 거의 모든 관례에서 기본값

- 문제점 : url에 모든 정보가 들어남

url에 정보를 담아서 보내기 때문에 args.get()으로 받을 수 있음

- 브라우저는 기본적으로 GET방식으로만 접근이 가능

<br>

<br>

### (2) Post

데이터를 보내거나 쓰는 요청

정보를 게시, 작성하는 요청 ex) 사진을 올려주세요

route()에 methods=['POST']로 명시해주어야 함

데이터를 숨겨서 보냄

\<form> 에 method=""로 명시해야 함

url(get방식)로 접근이 불가

**(flask)**request.form.get()으로 변수를 받음

<br>

<br>

<Br>

## II. Webhook

다른 서버와 데이터를 주고 받을 수 있는 모듈

웹 페이지의 기능을 알림으로 보내는 기능

메세지에 대해 서버가 응답하도록 구성할 수 있음

<br>

### (1) 기본 원리

서버가 telegram에 webhook설정을 요청

사용자가 telegram에 메세지를 보내면, telegram이 서버에 알림을 보냄

알림에 따라 서버가 반응

<br>

<br>

### (2) 사용 방법

C9에서 telegram의 setWebhook설정
- setWebhook을 사용하면 getUpdates를 사용할 수 없음
- 보낼 url을 설정해주어야 함
  - 기본 구조 : https://www.example.com/\<token>
- url로 알림과 보내진 메세지에 대한 정보가 json으로 POST방식에 의해 보내짐

url뒤의 route를 telegram 비밀번호로 설정 = 보안상의 문제

정상적으로 받을 경우 성공코드를 보내야 함

return시 response코드와 성공 코드를 같이 보내야 함

<br>

<br>

<br>


## III. 기타 내용

### (1) pprint

python에서 출력을 담당하는  기능

<br>

<br>





