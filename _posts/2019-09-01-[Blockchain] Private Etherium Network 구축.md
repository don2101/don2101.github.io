---
title: "Private Etherium Network 구축"
tags: ["Etherium"]
---



#### 하이퍼레저 패브릭 네트워크

허가형 블록체인, 기업형 블록체인.

완벽한 탈 중앙화 보다 기업 주도하에 시장, 비용의 효율성을 창출하는데 초점을 맞춘 블록체인

<br>

##### - 비허가형 블록체인 

참여자간 완전한 분산, 분권화를 지향하는 블록체인. 불특정 다수가 자유롭게 네트워크에 참여하여 블록체인을 구성

##### - 허가형 블록체인

비독점적인 이해 당사자들이 모여 하나의 컨소시움(연합)을 구성

따라서 네트워크 진입을 위해 다양한 인증 절차 필요하며 이를 통과해야 네트워크에 참여 가능

<br>

<br>

## Private Etherium Network 구축

<hr>

- etherium network 구축을 위해 테스트용 노드가 필요

- 테스트용 노드를 가상 머신을 활용해 구현하고 이를 etherium network로 연결한다.

<br>

### 1. 구축 도구

####  (1) virtual box(virtualization)

가상 서버 구축 도구. 

이더리움 네트워크를 구성하기 위해 여러대의 컴퓨터 노드를 한 컴퓨터 내에서 작동시킬 필요가 있다.

컴퓨터 하드웨어에 독립적인 리소스를 사용하여 다양한 의존성을 가진 sw 구동

<br>

####  (2) vagrant

가상 머신 provisioning 도구.

스크립트를 통해 virtual box 등의 가상 머신을 쉽게 관리할 수 있다.

사용자 요구에 따라 시스템 자원을 할당, 사용, 해제 등이 가능하다.

<br>

> 가상 환경 구축 예시

```bash
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

VAGRANTFILE_API_VERSION = "2"

# 가상 머신의 마지막 ip주소와, 할당할 메모리 크기
vms = {
  eth00: ['10', 2048],
  eth01: ['11', 2048],
  eth02: ['12', 1024],
  eth03: ['13', 1024],
  eth04: ['14', 1024],
}

# 반복문을 통해 가상 머신 구성
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/bionic64"
  vms.map do |key, value|
    name = key.to_s
    ip_num, mem = value
    config.vm.define "#{name}" do |node|
      node.vm.network "private_network", ip: "192.168.50.#{ip_num}"
      node.vm.hostname = "#{name}"
      node.vm.provider "virtualbox" do |nodev|
        nodev.memory = "#{mem}"
      end
    end
  end
end
```

<br>

#### (3) Geth

network 구축에 있어 참여자(클라이언트)역할을 할 노드들이 구성되어야 한다.

etherium 클라이언트를 구축하는 Go 언어로 개발된 프로그램

이더리움에서 제공하는 다양한 API를 사용할 수 있다.

<br>

<br>

### 2. Geth 클라이언트 구성

> bash 창에서 가상 머신에 ssh로 접속

```bash
vagrant ssh "가상 머신 이름"
```

<br>

> 가상 머신상에서 geth 설치

```bash
sudo apt-get install software-properies-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum

# geth 버전 확인
geth version
```

<br>

> Genesis 블록 생성

```bash
# ~/dev/pri_eth/CustomGenesis.json

{
    "config": {
        "chainId": 15150,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "alloc"                 : {},
    "coinbase"              : "0x0000000000000000000000000000000000000000",
    "difficulty"    : "0x10",
    "extraData"             : "",
    "gasLimit"              : "9999999",
    "nonce"                 : "0xdeadbeefdeadbeef",
    "mixhash"     : "0x0000000000000000000000000000000000000000000000000000000000000000",
    "parentHash"  : "0x0000000000000000000000000000000000000000000000000000000000000000",
    "timestamp"             : "0x00"
}
```

#### genesis 블록

블록체인의 0번째 블록

Genesis파일(.json)에 Genesis 블록의 정보를 저장한다

<br>

>Genesis 블록 생성

etherium network 구축을 위해 최초의 블록이 필요

```bash
geth --datadir /home/vagrant/dev/pri_eth/ init /home/vagrant/dev/pri_eth/CustomGenesis.json
```

<br>

>가상 머신 상에서 geth 구동

etherium network를 구축할 geth 클라이언트를 실행

```bash
# eth00(부트 노드)
geth 
--networkid 15150 
--datadir ~/dev/pri_eth # genesis 블록이 존재하는 위치
--port 30303 # etherium 전용 newtwork port
--rpc --rpcport 8545 --rpcaddr 0.0.0.0 --rpccorsdomain "*" 
--rpcapi "admin,net,miner,eth,rpc,web3,txpool,debug,db,personal" console

# 나머지 노드
geth 
--networkid 15150 
--maxpeers 5 
--datadir ~/dev/pri_eth 
--port 30303 console
```

<br>

> geth node 정보 확인

```bash
# 다른 노드에서 add peer에 사용
amdin.nodeInfo.enode
> "enode://1d ~~~ 34@127.0.0.1:30303"
```

<br>

> network 구축을 위한 node 연결

```bash
admin.addPeer("enode 주소")
```

<br>

> 연결 확인

```bash
admin.peers
```

<br>

#### 연결을 편하게 하는 법

> CustomGenesis.json이 존재하는 위치에 다음과 같은 이름으로 json파일 저징

```bash
# static-nodes.json

["enode 주소1". "enode 주소2", ...]
```

그러면 geth 실행시 자동으로 연결

<br>

<br>

## Smart Contract 구축

<hr>

### 개발 도구

#### (1) Remix

브라우저 상에서 이용 가능한 스마트 컨트랙트 개발 도구(IDE).

<br>

#### (2) solidity

컴파일 가능하며 스마트 컨트랙트를 테스트, 배포할 수 있다.

<br>

#### (3) Metamask

이더리움 지갑 프로그램

<br>

#### (4) ethereumjavascript api(web3.js)

이더리움 네트워크에 접속하고, 웹 페이지상에서 보여주는 도구

<br>

테스트 넷: 실제 블록체인 네트워크에 적용시키기 전에 테스트하는 환경. 임시 네트워크

메인 넷: 실제 블록체인을 배포하고 운영하는 환경

데이터 디렉토리: 송수신한 블록 데이터와 계정 정보 저장
















# genesis 블록 생성



# genesis 블록 설정
{
        "config": {
                "chainId": 15150,
                "homesteadBlock": 0,
                "eip155Block": 0,
                "eip158Block": 0
        },
        "alloc"                 : {},
        "coinbase"              : "0x0000000000000000000000000000000000000000",
        "difficulty"    : "0x10",
        "extraData"             : "",
        "gasLimit"              : "9999999",
        "nonce"                 : "0xdeadbeefdeadbeef",
        "mixhash"               : "0x0000000000000000000000000000000000000000000000000000000000000000",
        "parentHash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
        "timestamp"             : "0x00"
}


# Geth 구동
geth --networkid 15150 --datadir ~/dev/pri_eth --port 30303 --rpc --rpcport 8545 --rpcaddr 0.0.0.0 --rpccorsdomain "*" --rpcapi "admin,net,miner,eth,rpc,web3,txpool,debug,db,personal" console


# 나머지 서버 구동
geth --networkid 15150 --maxpeers 5 --datadir ~/dev/pri_eth --port 30304 console


# node 정보 화인


# node에 연결

