---
title : "Chatbot Project 2nd"
tags: ["Chatbot", "Scrap"]
---

# Python Chatbot 2 일차

#### 2018. 12. 18(Tue)

Scrap 배우기

[수업Github 주소](github.com/sspy1)

이해가 안된다?

1. 논리적으로
2. 익숙하지 않다 => 패턴을 학습하며 익숙해진다

어떤 기술이든지 API문서를 파악할 것

<br>

<br>


## I. Scrap

웹 페이지의 id, class 명을 통해 자료를 긁어온다

반복되는 내용중 내가 선택하고자 하는 내용을 캐치할 수 있는 능력이 중요

<br>

<br>

### (1) 원리

API 혹은 웹페이지에 존재하는 json, html문서를 requests를 통해 불러온다

불러온 문서를 bs4를 통해 python이 이해하기 쉽게 parsing

- parsing : 의미 없는 자료를 언어적으로 의미가 있는 자료로 바꾸는것

parsing한 자료에서 select, select_one등의 메소드를 사용하여 선택한 id, class에 접근

선택한 내용을 text로 추출

<br>

<br>

### (2) bs4

Scrap과 parsing을 쉽게 할 수 있도록 도와주는 툴

Scrap을 위한 다양한 기능이 존재

트리구조로 된 html문서를 구조 그대로 가져온다

Json은 문서파일(모든 것이 문자이다 => key값으로 value에 접근이 불가능) != dictionary

=> Json을 dictionary로 파싱이 가능!

<br>

<br>

### (3) select, select_one

select : id나 class 명으로 요소와 태그를 리스트로 가져오는 연산

select_one : 요소 하나와 태그를 가져오는 연산

<br>

<br>

## III. 기타 기록

### (1) 언급한 기술들

jira => 애자일 방법으로 프로젝트 관리

bitbucket

ifttt(if this then that)

셀레니움 : 무엇이든지 긁어올 수 있는 패키지

<br>


>Traceback (most recent call last):                                                                                                                                                                                                                                             
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\urllib3\connectionpool.py", line 600, in urlopen                                                                                                                                             
>chunked=chunked)                                                                                                                                                                                                                                                           
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\urllib3\connectionpool.py", line 343, in _make_request                                                                                                                                       
>self._validate_conn(conn)                                                                                                                                                                                                                                                  
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\urllib3\connectionpool.py", line 839, in _validate_conn                                                                                                                                      
>conn.connect()                                                                                                                                                                                                                                                             
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\urllib3\connection.py", line 364, in connect                                                                                                                                                 
>_match_hostname(cert, self.assert_hostname or server_hostname)                                                                                                                                                                                                             
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\urllib3\connection.py", line 374, in _match_hostname                                                                                                                                         
>match_hostname(cert, asserted_hostname)                                                                                                                                                                                                                                    
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\ssl.py", line 323, in match_hostname                                                                                                                                                                       
>% (hostname, ', '.join(map(repr, dnsnames))))                                                                                                                                                                                                                              
>ssl.SSLCertVerificationError: ("hostname 'www.nlotto.co.kr' doesn't match either of '*.dhlottery.co.kr', 'dhlottery.co.kr'",)                                                                                                                                                  
>During handling of the above exception, another exception occurred:                                                                                                                                                                                                            
>Traceback (most recent call last):                                                                                                                                                                                                                                             
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\requests\adapters.py", line 449, in send                                                                                                                                                     
>timeout=timeout                                                                                                                                                                                                                                                            
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\urllib3\connectionpool.py", line 638, in urlopen                                                                                                                                             
>_stacktrace=sys.exc_info()[2])                                                                                                                                                                                                                                             
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\urllib3\util\retry.py", line 398, in increment                                                                                                                                               
>raise MaxRetryError(_pool, url, error or ResponseError(cause))                                                                                                                                                                                                             
>urllib3.exceptions.MaxRetryError: HTTPSConnectionPool(host='www.nlotto.co.kr', port=443): Max retries exceeded with url: /common.do?method=getLottoNumber&drwNo=836 (Caused by SSLError(SSLCertVerificationError("hostname 'www.nlotto.co.kr' doesn't match either of '*.dhlott
>ery.co.kr', 'dhlottery.co.kr'")))                                                                                                                                                                                                                                              
>During handling of the above exception, another exception occurred:                                                                                                                                                                                                            
>Traceback (most recent call last):                                                                                                                                                                                                                                             
>File "lotto.py", line 17, in                                                                                                                                                                                                                                         
>jsonFile = requests.get(url)                                                                                                                                                                                                                                               
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\requests\api.py", line 75, in get                                                                                                                                                            
>return request('get', url, params=params, **kwargs)                                                                                                                                                                                                                        
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\requests\api.py", line 60, in request                                                                                                                                                        
>return session.request(method=method, url=url, **kwargs)                                                                                                                                                                                                                   
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\requests\sessions.py", line 533, in request                                                                                                                                                  
>resp = self.send(prep, **send_kwargs)                                                                                                                                                                                                                                      
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\requests\sessions.py", line 668, in send                                                                                                                                                     
>history = [resp for resp in gen] if allow_redirects else []                                                                                                                                                                                                                
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\requests\sessions.py", line 668, in                                                                                                                                                
>history = [resp for resp in gen] if allow_redirects else []                                                                                                                                                                                                                
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\requests\sessions.py", line 247, in resolve_redirects                                                                                                                                        
>**adapter_kwargs                                                                                                                                                                                                                                                           
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\requests\sessions.py", line 646, in send                                                                                                                                                     
>r = adapter.send(request, **kwargs)                                                                                                                                                                                                                                        
>File "C:\Users\student\AppData\Local\Programs\Python\Python37\lib\site-packages\requests\adapters.py", line 514, in send                                                                                                                                                     
>raise SSLError(e, request=request)                                                                                                                                                                                                                                         
>requests.exceptions.SSLError: HTTPSConnectionPool(host='www.nlotto.co.kr', port=443): Max retries exceeded with url: /common.do?method=getLottoNumber&drwNo=836 (Caused by SSLError(SSLCertVerificationError("hostname 'www.nlotto.co.kr' doesn't match either of '*.dhlottery.
>co.kr', 'dhlottery.co.kr'")))                                                                                                                                                                                                                                                  `
>
>

=> 보내는 url주소의 인증서가 만료되어 새로운 도메인으로 바뀌었을때 발생하는 오류


