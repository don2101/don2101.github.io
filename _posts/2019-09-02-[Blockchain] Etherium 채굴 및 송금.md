---
title: "Etherium 채굴 및 송금"
tags: ["Etherium"]
---



## 채굴

<hr>

### 1. 계정 관련 메서드

**personal.**

채굴을 하기 전에 해당 노드에서 계좌(계정)를 생성해야 한다.

<br>

> 계정 생성

```bash
personal.newAccount("비밀번호")
```

- 해당 이름으로 입력 시 "비밀번호를" 가진 account 생성

```bash
> personal.newAccount("12345")
INFO [08-20|09:43:50.299] Your new key was generated               address=0x426f97...89c26a
WARN [08-20|09:43:50.299] Please backup your key file!             path=/home/vagrant/dev/pri_eth/keystore/UTC--2019-08-20T09-43-49.006056982Z--426f97af...9c26a
WARN [08-20|09:43:50.299] Please remember your password!
"0x426f97...9c26a"
```

<br>

> 계정 확인

```bash
personal.listAccounts
```

계좌는 여러 개 생성할 수 있으며 한 노드의 `0번째` 계좌에 코인이 들어간다.

<br>

### 2. 블록 관련 메서드

**.eth**

> 계정 확인

```bash
> eth.accounts
["0x8610...22347b", "0x65d...f55d9", "0x426f...89c26a"]
```

- 리스트이므로 index 접근 가능

```bash
> eth.accounts[1]
"0x65d...f55d9"
```

<br>

#### Etherbase

코인을 채굴하여 보상을 받은 계정

기본적으로 [0] 번째 index의 계좌

<br>

> 보상 계좌 확인

```bash
> eth.coinbase
"0x8610...22347b"
```

<br>

> 해당 계좌 잔고 확인

```bash
eth.getBalance("계정 id")

> eth.getBalance(eth.accounts[0])
0
```

<br>

> 블록 체인의 블록 수

```bash
eth.blockNumber
```

<br>

> 채굴중인지 확인

```bash
eth.mining
```

<br>

> 채굴 속도 확인

```bash
eth.hashrate
```

<br>

### 3. 채굴 관련 메서드

**miner**

> 채굴 보상 계좌 변경

```bash
miner.setEtherBase(eth.accounts[0])
```

<br>

> 채굴 시작

```bash
miner.start("스레드 수")
```

- "스레드 수"에 채굴에 할당할 스레드의 수를 정할 수 있다.
- 채굴이 시작되면 네트워크로 연결된 모든 노드에 채굴 상황이 공유된다.
- 따라서 test 환경에서 동시에 bash창을 켜놓으면 다른 노드에서도 채굴 상황이 진행되는 것을 볼 수 있다.

<br>

> 채굴 중지

```bash
miner.stop()
```

- 채굴이 진행되는 동안은 채굴을 중지할 수 없으며, 하나의 채굴이 완료되면 중지된다.
- 채굴이 완료되면 보상받는 계좌에 코인이 쌓이게 된다.





