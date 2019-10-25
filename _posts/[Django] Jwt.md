django에서 사용하는 인증, 로그인 방식과 jwt를 사용하는 방식을 비교한다





## 1. 계정 정보를 헤더에 넣는 방식

- 계정 정보를 http 헤더에 담아 요청하는 방식



#### 단점

- 데이터를 요청할 때 마다 사용자 개인 정보를 담아 보내는 것은 보안상 아주 취약하다
- 또한 서버에서 요청이 올 때 마다 id, pw를 통해 인증을 해야 하므로 매우 비효율적





## 2. Session / Cookie 방식

### 작동 순서

1. 사용자가 로그인을 요청
2. 서버에서 사용자를 확인한 뒤, 회원정보를 **세션저장소**에 저장하고 사용자에게 고유한 ID값을 부여
3. 서버에서 세션 ID를 사용자에게 전송하고 사용자는 이를 Cookie에 저장
4. 이후 인증이 필요한 상황마다 쿠키를 헤더에 실어서 전송
5. 서버에서 쿠키에 저장된 데이터를 세션저장소에 있는 데이터와 비교하여 사용자를 인증
6. 인증이 완료되면 사용자에게 정보를 제공



- 기본적으로 세션저장소를 필요로 한다
  - ex) Redis



### Session vs Cookie

- Session: 서버에서 사용자의 정보를 저장하는 저장소
- Cookie: 사용자(브라우저)측에서 서버에서 제공된 정보를 저장하는 공간



#### 단점

- 쿠키만으로 인증한다는 것은 서버의 자원을 사용하지 않는다는 것
- http가 탈취되면 쿠키 또한 탈취되며, 올바르지 않은 사용자의 접근이 가능하다
- 세션저장소로 인해 서버측의 부하가 높다





## 3. 토큰(JWT)기반 인증 방식

- 인증에 필요한 정보를 암호화 해서 보내는 방식
- Session / Cookie방식과 유사하며, 사용자는 token을 http 헤더에 실어 보낸다



> 예시

[https://jwt.io](https://jwt.io/)



### 구조

토큰을 구성하는 요소

- Header: 암호화 할 알고리즘을 명시하는 부분
- Payload: 서버에서 보낼 데이터가 담기는 부분. 보통 id, 유효 기간 등을 명시한다
- Verify Signature: Base64 방식으로 인코딩한 Header, payload 그리고 secret key를 더한 후 암호화



#### 최종으로 만들어 지는 token 결과

`Encoded Header` + `Encoded Payload` + `Verify Signature`



#### Verify Signature

- 결국 Verify Signature에는 Header와 Payload의 정보가 담긴다

`Verify Signature` = `Encoded Header` + `Encoded Payload` + `secret key`

- 복호화 할 때는 secret key가 필요하며 이는 서버에서 지정한다



### 특징

- Header와 Payload는 인코딩 될 뿐 **암호화 되지 않는다**
- 따라서 누구나 디코딩 하면 정보를 확인할 수 있다(중요 정보를 payload에 담지 않는다)
- Session / Cookie 방식과 다르게 악의적인 사용자가 token을 가로챈다 하더라도 인가를 받을 수 없다
  - 악의적인 사용자가 Payload의 id정보를 자신의 id로 변경하면, Verify Signature의 값이 변경되기 때문에 유효한 사용자가 아님을 판단할 수 있다
- token 안에 유저의 정보를 담기 때문에 서버에서 별도의 저장소가 필요하지 않다



### 작동 순서

1. 사용자가 로그인을 요청
2. 서버에서 사용자를 확인한 뒤, 여러 정보(id, expired date...)를 Payload에 포함
3. secret key를 통해 정보를 암호화 하고 jwt를 생성 한 뒤, 사용자에게 전송
4. 사용자는 jwt를 session storage등에 저장하고, 인증이 필요할 때 마다 jwt를 전송
5. 서버에서 jwt의 Verify Signature를 secret key를 통해 복호화 한 뒤, 조작 여부, 유효 기간을 판별
6. 인증이 완료되면 사용자에게 정보를 제공









### 참고 사이트

**https://tansfil.tistory.com/58**