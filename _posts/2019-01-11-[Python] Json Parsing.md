---
title: Python Json parsing
tags: ["Python", "Json"]
---



## Json parsing

<hr>

= input() : 표준 입력 받기

<br>

####python json parsing

- .json() 사용
  - requests.get(url)을 response 변수에 저장한 후
  - response.json()을 사용해 json 형식으로 변환
- import json 사용
  - requests로 불러온 결과를 text로 변환
  - json.loads(text)를 사용해 json으로 변환

- json에서 .get('태그')로도 접근이 가능
  - 없을 경우 그냥 넘어감

