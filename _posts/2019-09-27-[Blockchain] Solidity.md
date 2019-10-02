---
title: "Solidity"
tags: ["Solidity", "Smart Contract"]
---



## 스마트 컨트랙트

<hr>

#### 비트코인의 발생 이후로...


- 디지털 통화 개념의 생성
- 탈 중앙화 화폐 시스템의 등장

<br>

#### 그래서 이더리움은...

- 비트코인의 탈중앙화 화폐 시스템 위에 더 많은 기능을 추가하고자 한다

<br>

### 1. 스마트 컨트랙트

- 튜링 완전한 프로그래밍 언어를 사용하여 코드 상에서작동하는 계약
- 이더리움 상에서 코드로 계약에 관한 내용을 작성하면 이벤트에 따라 계약이 실행된다

<br>

## Solidity

<hr>

스마트 컨트랙트(계약)관련 내용을 코드로 구현한 것

이더리움에서 개발한 스마트 컨트랙트 개발 언어. 

바이트 코드로 변환되어 가상 머신 위에서 동작. 

튜링 완전(계산적인 문제를 프로그래밍 언어로 풀 수 있는)하다

<br>

### 1. 필수 개념

#### (1) 버전 선언

- 지원하는 솔리디티 버전을 선언

```solidity
pragma solidity ^0.4.24;
```

<br>

#### (2) Function Modifier

- 선언적 방법로 함수의 조각, 행동을  통해 수정하는데 사용
  - ex) 함수의 실행 전에 상태를 먼저 체크
- contract의 상속 가능한 변수이며, 다른 contract에서 override 가능
- 함수를 실행하기 전에 조건을 걸어 필터링

<br>

> 적용 예시

```solidity
pragma solidity ^0.4.22;

contract owned {
    function owned() public { owner = msg.sender; }
    address owner;

    modifier onlyOwner {
        require(
            msg.sender == owner,
            "Only owner can call this function."
        );
        _;
    }
    
contract Register is priced, owned {
    uint price;
	
	// changePrice를 실행하기 전에 조건 검사
    function changePrice(uint _price) public onlyOwner {
        price = _price;
    }
}
```

- `changePrice`를 실행하기 전에 `onlyOwner`의 조건을 실행
- `require()` 안의 `msg.sender == onwer` 조건이 성립해야 함수 실행

<br>

#### (3) Fallback function

- contract가 가지는 하나의 이름없는 함수이며 contract에 의해서만 실행된다.
- 인자를 받을 수 없으며, return값도 없고, `external` visivility를 갖는다.
- fallback function은 contract가 ehter를 수신할 때 마다 실행된다.

<br>

#### (4) payable

- fallback function이 ether를 수신하고, 총 balance에 추가하기 위해 해당 fallback이 ehter를 수신 받을 수 있다는 것을 명시해야 한다.
- `payable` 옵션을 함수에 명시함으로써 이를 표기한다.
- 만약 contract에 `payable`옵션을 가진 함수가 없다면 contract는 transaction으로 부터 ether를 받을 수 없고, 예외를 던진다. 

<br>

> 적용 예시

```soliditiy
contract Sink {
    function() external payable { }
}
```

<br>

### Solidity 통신 방법

트랜잭션을 통해 이더리움 서버에 있는 함수 호출

- web3j: Java언어로 송수신
- web3js: Javascript로 송수신

<br>

구조

https://goodjoon.tistory.com/261?category=632200

이벤트

https://www.pauric.blog/How-to-Query-and-Monitor-Ethereum-Contract-Events-with-Web3/

