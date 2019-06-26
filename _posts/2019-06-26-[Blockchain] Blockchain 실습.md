---
title: "Blockchain 적용 예시"
tags: ["Blockchain", "Ethereum"]
---



# 블록체인 적용 예시

##### 2019. 06. 26 (Wed)

<br>

## Private Ethereum 구축

#### 필요 도구

- VirtualBox
- Vagrant
- VSCode

<br>

### 1. 가상 환경 구축

#### 가상머신 실행

##### 현재 directory에 vagrant 가상머신 설정

```bash
vagrant init
```

<br>

##### Vargrantfile 설정을 통한 가상머신 생성

```
Vagrant.configure("2") do |config|
  config.vm.define "eth01" do |eth01|
    eth01.vm.box = "ubuntu/bionic64"
    eth01.vm.hostname = "eth01"
    eth01.vm.network "private_network", ip: "192.168.56.121"
    eth01.vm.provider "virtualbox" do |eth01v|
      eth01v.memory = 4096
    end
  end
  config.vm.define "eth02" do |eth02|
    eth02.vm.box = "ubuntu/bionic64"
    eth02.vm.hostname = "eth02"
    eth02.vm.network "private_network", ip: "192.168.56.122"
    eth02.vm.provider "virtualbox" do |eth02v|
      eth02v.memory = 4096
    end
  end
end
```

<br>

##### 생성한 가상머신 구동

```bash
vagrant up eth01(or eth02)
```

- 각각의 bash에서 동시에 진행시 오류 발생

<br>

##### ssh로 가상머신 접속

```bash
vagrant ssh eth01
```

<br>

##### Geth(Got-ethereum client) 설치

```bash
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get install ethereum
```

<br>

##### 설치 확인

```bash
geth version
```

<br>

<br>

### 2. Ethereum 환경 구축

#### private 이더리움 위한 genesis 블록 생성

##### 가상 머신 내에서 수행

```bash
mkdir -p dev/eth_localdata
cd dev/eth_localdata

# genesis블록 생성 json
vi CustomGenesis.json
```

<br>

##### CustomGenesis.json

```json
{
  "config": {
    "chainId": 921,
    "homesteadBlock": 0,
    "eip155Block": 0,
    "eip158Block": 0
  },
  "alloc": {},
  "coinbase": "0x0000000000000000000000000000000000000000",
  "difficulty": "0x20",
  "extraData": "",
  "gasLimit": "0x47e7c5",
  "nonce": "0x0000000000000042",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp": "0x00"
 }
```

<br>

#### 제네시스 블록

블록체인에서 최초로 생성되는 블록

앞에 연결되어있는 블록이 없다.

<br>

##### Geth 초기화

```bash
geth --datadir /home/vagrant/dev/eth_localdata init /home/vagrant/dev/eth_localdata/CustomGenesis.json
```

<br>

##### Geth 구동

geth console로 접근

```bash
geth --networkid 921 --maxpeers 2 --datadir /home/vagrant/dev/eth_localdata --port 30303 console
```

- port 30303: 보통 이더리움에서 사용하는 port

<br>

> eth02에서는 다른 port를 사용한다

```bash
geth --networkid 921 --maxpeers 2 --datadir /home/vagrant/dev/eth_localdata --port 30304 console
```

**eth02도 동일하게 실행**

<br>

#### 노드 연결

##### 노드 정보 확인

```bash
# eth01
> admin.nodeInfo.enode
"enode://.....@[xxx.xxx.xxx.xxx:30303]"
```

<br>

##### 노드 접속

```bash
# eth02
> admin.addPeer("enode://.....@[xxx.xxx.xxx.xxx:30303]") # eth01 노드 정보
```

<br>

##### 노드 연결 확인

```bash
# eth01
> admin.peers
```

<br>

#### 계정 설정

##### 이더리움 계정(EOA) 생성

```bash
# eth01
> personal.newAccount("test123") # 여기서 "test123"은 account의 비밀번호가 된다
"0xab835..."
```

<br>

##### 생성된 계정 확인

```bash
> eth.accounts
```

이후 eth02에서도 동일하게 실행

<br>

<br>

### 3. 트랜잭션 작업

#### 채굴

##### 트랜잭션 생성을 위한 ETH 채굴

```bash
> miner.start(1)
true
```

- percentage로 진행상황이 출력된다.
- 하나의 채굴이 끝나야 종료 가능

<br>

##### 채굴 종료

```bash
> miner.stop()
true
```

<br>

##### 채굴 보상으로 획득한 ETH 잔액 확인

```bash
> eth.getBalance("계정id")
0...
```

<br>

#### 트랜잭션 전송

##### 트랜잭션 생성, 전송

```bash
# eth01
> eth.sendTransaction({from: "계정id", to: "받는 계정id", value: web3.toWei(1, "ether")})
```

- 그러나 트랜잭션 전송시 인가받지 않은 유저일 경우 전송이 안 될 수 있다.

<br>

##### 유저 인증

```bash
# 15000 mili second 이내에 list중 [0]번째에 있는 user를 인증
# <password>에는 계정 생성시 입력한 password를 삽입한다.
web3.personal.unlockAccount(web3.personal.listAccounts[0],"<password>", 15000)
```



