---
title : "Chatbot Project 3rd"
tags: ["Web", "Flask"]
---

# Python Chatbot 3 일차

#### 2018. 12. 19(Wed)

c9 서비스 사용

하나의 프로그램, 파일, 모듈에 하나의 로직만

![](https://l3snh1odsii3hndbd2rovotm-wpengine.netdna-ssl.com/wp-content/uploads/2017/12/cloud9_logo.png)

<br>

## I. C9

AWS에서 제공하는 서비스

클라우드 컴퓨팅 기반으로 웹 서비스를 제공하는 서버를 구축

기본적인 개발 환경, IDE를 제공

웹상에서 쉽게 편집을 할 수 있으며, 터미널을 통해 실행하고 서버를 띄울 수 있음

<br>

### (1) 구조

파일트리, 터미널, 개발 환경으로 구성

<br>

<br>

<br>

## II. Web

### (1) Flask

Spring boot와 유사한 구조

간단함에 중점을 둔 **python framework**

<br>

send_file(`html문서`)

- 파일을 전송하는 기능
- python에서 html파일을 웹 브라우저에 전송

<br>

render_template(`html문서`, `html변수=python변수`, ...)

- python의 변수, 결과들을 html문서에 전달
- 동적으로 page 결과를 생성
- html이 들어가는 기본 폴더 이름은 'templates'

index() 는 convention으로써 사용

<br>

app = Flask(\__name__) : Flask 앱을 하나 만든다

<br>

<br>

### (2) Web과 Url

web 요청은 기본적으로 **url을 통해**

요청의 본질은 url

- url의 구조
  1. protocol : http
  2. sub-domain : www
  3. domain : naver - 수신자의 주소
  4. / : 가장 기본이 되는(root) 페이지
     - / : root directory (windows - c:/)
  5. / 이후 내용 : 무엇을 받을지
     - ? 이후 내용 : 송신자가 담아 보내는 내용(parameter)
     - parameter의 기본은 string
     - 반환도 string으로 해야 한다
     - parameter를\<int:num>으로 하면 형변환이 됨
     - 여러개일 경우 &에 추가하여 작성

ex) https://www.naver.com/search.naver?query=hello&text=world

routing : 어떤 주문을 받아 어떤것을 줄 지 정하는 것

<br>

<br>

### (3) html

모든 html파일은 tag로 이루어짐

web 문서의 뼈대

hyper text : 링크를 가진 문서

markup : 태그를 이용하여 정의하는 특성

html/css만으로는 static page(항상 동일한)밖에 작성할 수없다.

=> html안에 python 코드를 심어 동적으로 처리한다!

=> 프레임워크의 도움을 받는다. Rails(Ruby), Django(Python), Node.js(Javascript)

<br>

#### 기본 문법

모든 tag는 문서 내에서 각자의 공간을 차지

\<!DOCTYPE html> : 문서를 html으로 정의

##### vscode. 잡기술 : !, tab으로 빠르게 작성 가능

\<html> : 전체 틀을 구성하는 tag

\<head> : 머리 부분 tag

- \<title> : 문서의 제목이 들어가는 tag

\<meta charset="utf-8"> : 한글 인식용 tag

\<body> : 몸통. 대부분의 내용이 들어가는 tag
- \<h1> : 제목. 가장 중요한 내용.  - 구글이 주로 크롤링 하는 내용
  - \<h6> 까지 가능
- \<p> : 그냥 내용. 문서안의 공간을 좀 더 차지
- \<div>  : 그냥 내용. \<p>와 동일하지만 공간을 덜 차지

\<a> : 링크를 표현하는 태그

- href="" : 담겨진 링크

\<img> : 이미지를 담는 태그
- src="" : 이미지의 링크
- alt="" : 대신 보여줄 이미지, 혹은 text 

\<table> : 테이블을 만듦
- \<tr> : table row. 테이블의 행
- \<td> : table data. 테이블의 열
  - vs 잡기술 : table > tr\*3 > td\*2

\<ul> : unordered list. 순서없는 리스트

\<ol> : 순서 있는 리스트

id : 태그마다 붙여진 고유한 식별자

class : 태그마다 갖고 있는 고유한 속성

<br>

#### Web 표준

- seo : 검색 엔진 최적화 된 앱을 만들어야 한다(좋은 서비스를 만드는 요소)
- html은 검색이 됐을 때가 가장 중요함
  - 구글 검색에서 크롤링이 잘 되도록

<br>

<br>

<br>

## III. css

html을 꾸미는 문서

일일이 다 할 수 없기 때문에 template를 사용

<br>

#### 기본 문법

style="color: {색}"

\<style> : 선택자, tag에 따라 스타일을 적용

class 선택 시 . 사용

id 선택시 # 사용

<br>

#### 유용한 Template

- Bootstrap
- css-trick
- css flex : css 정렬용

<br>

<br>

<br>

## IV. 기타 내용

### (1) Python 문법

- python string reversing
  1. reversed() 방법 : ''.join(reversed(str))
  2. slicing 방법 : str = str[::-1]

<br>

### (2) 좋은 MOOC

1. Coursera(미국 서부)
2. cs50(미국 동부)
3. W3
4. UDACITY(현업에서 쓰일 수 있는 강의) - 통계학, Machine Learning

<br>

### (3) 언급된 기술

1. waymo
2. jinja
3. jekill : github 페이지를 동적으로 제작

<br>

### (4) Github Pages

- 페이지 관리 및 호스팅 기능
- -.github.io형식으로 repository 생성
- 적용 시키는데 시간이 조금 걸린다
- index.html 파일을 자동으로 찾아 호스팅
- 프로필, 개발 문서 관리용으로 적합