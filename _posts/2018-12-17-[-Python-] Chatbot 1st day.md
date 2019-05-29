# Python Chatbot 1 일차

#### 2018. 12. 17(Mon)

- 기능을 익힐 때 들어가는 값이 무엇이고 나오는 값은 무엇인가?(abstraction)를 파악하기

- API(공식문서)를 통해 기능을 빠르게 숙지

<br>

<br>

## I. 개발 계명(Development Commandments)

### (1) 브라우저는 크롬이다

### (2) 문서는 마크다운(.md)이다

### (3) 교과서는 공식문서 & 내가 정리한 마크다운이다.

<br>

<br>

<br>

## II. Web API

#### interface?

다른 시스템 간의 **커뮤니케이션 방식**.

소통할 수 있는 통로.

<br>

#### API

API : **프로그래밍을 통해** 인터넷의 자료, 서비스에 접근 => 프로그래밍을 통해 정보 접근

사용 방법은 몰라도 된다. 그저 사용하기만.

<br>

#### request and response

request : url로 정보를 요청. 클라이언트에서 보내는 요청

response : 서버에서 보내는 클라이언트의 요청에 대한 응답(html 문서) => **그냥 문서**를 받는 것 뿐

<br>

요청자(`url`) <==> 응답(문서 - `xml, html, json`)

<br>

#### 요청을 보내는 방법

http://~~~~~~

?

sidoName=서울

&

pageNo=3

&

ServiceKey=<secrete_api_key>

<br>

#### python에서 제공되는 접근 방법

```python
# 기본 웹브라우저로 url 오픈
webbrowser.open(url)
```

<br>

##### bs4 사용

```python
#response로 온 html문서를 python이 이해하기 쉽게 정제
bs4.BeutifulSoup(text)
```

- select_one(): tag를 기준으로 요소를 선택한다.

<br>



