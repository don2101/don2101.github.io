## Python is vs '==' operator

##### 2019. 01. 04 (Fri)



- is : 레퍼런스 체크(참조 비교)
- == : 값 비교

```python
number = 1
number == 1
>>> True

number is 1
>>> True
```

- python은 내부에 -5 부터 256 까지 숫자를 배열로 캐싱해서 사용한다.
- 즉, -5 ~ 256을 어떤 변수에 담더라도 같은 레퍼런스를 가리킨다.
- 따라서, number와 1의 레퍼런스가 같은것으로 판단한다.

```python
number = 257
number == 257
>>> True

number is 257
>>> False
```

- 256을 초과할 경우 다른 레퍼런스로 판단한다.



#### 레퍼런스 비교

```python
# 같은 레퍼런스
number = 256
id(number)
>>> 140718943359792
id(256)
>>> 140718943359792

#다른 레퍼런스
number = 257
id(number)
>>> 1910484854128
id(257)
>>> 1910484854160
```



#### 함수의 경우

```python
def referenceCheck() :
	number = 257
	print(number is 257)
    
referenceCheck()
>>> True
```

- 하지만, 함수 내부에서는 같은 레퍼런스를 가리킨다.



#### String의 경우

- string에 한글이 포함되어 있을 경우 다른 레퍼런스로 인식한다.
- 영어와 한글은 같은 레퍼런스로 인식한다.
- 공백, 기타 문자들 또한 포함될 경우 다른 레퍼런스로 인식한다.

```python
s = "안녕하세요"
s is "안녕하세요"
>>> False

s = "Helloworld"
s is "Helloworld"
>>> True
```



