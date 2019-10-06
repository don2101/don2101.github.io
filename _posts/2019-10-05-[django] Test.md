---
title: "Django test"
tags: ["Django", "Python", "Test"]
---



## Django에서 Test 수행

python에서 `unittest`라이브러리를 제공하며, 이를 사용하여 단위 테스트를 진행할 수 있다.

[파이선 단위 테스트](https://don2101.github.io/2019/06/10/Python-Unit-Test/)



django에서는 `django.test` 라이브러리를 통해 단위 테스트를 수행할 수 있다.



### 1. 테스트 클래스 생성

```python
from django.test import TestCase
import requests

class PostTest(TestCase):
    # test가 진행되기 전에 설정
    # 로그인 로직을 수행하고 post_body 구성
    def setUp(self):
        self.LOGIN_URL = "http://localhost:8000/accounts/signin/"
        self.login_data = {
            "username": "aaa",
            "password": "qwe",
        }

        self.POST_URL = "http://localhost:8000/"
        self.post_body = {
            "title": "goodmanwfewfwe",
            "content": "contentwefewf",
        }

        result = requests.post(self.LOGIN_URL, data=self.login_data)
        
        self.post_body["token"] = result.text[1:len(result.text)-1]

    def test_post(self):
        # post를 올리는 테스트
        result = requests.post(self.POST_URL, data=self.post_body)

        self.assertEqual(result.status_code, 201)
```

- login요청을 하여 jwt를 받아 post를 올리는 테스트
- `test.py` 에서 제공되는 TestCase를 상속받아 사용
- `unittest` 와 유사하게 setUp을 구성하고 test를 진행
- post를 올리고 status code가 201이면 test 성공



### 2. test 수행

#### test 시작

```bash
python3 manage.py test
```

- 프로젝트 내에 존재하는 모든 `test.py`를 수행



> post 결과

![](https://user-images.githubusercontent.com/19590371/66265346-ea271100-e84f-11e9-8c75-60d1d031b447.png)







