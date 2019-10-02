---
title: "지갑"
tags: ["Etherium"]
---



### 지갑

- Etherium Network와 사용자를 이어주는 인터페이스
- 사용자는 Etherium Network에 저장된 계좌와 API를 사용하여 etherium 결제, 송금 등의 작업을 한다.
- 그러나 사용자가 이런 계좌와 API에 직접 접근을 하면 안되므로, back-end와 front-end에서 Etherium Network에 접근할 수 있는 인터페이스가 필요
- 지갑은 back-end와 front-end에서 구현된 Etherium Network에 접근할 수 있는 인터페이스 이며, 사용자는 이러한 인터페이스(지갑)을 통해 etherium 관련 작업을 할 수 있다.

<br>

> front-end 서버

```javascript
// web3 객체 생성
var web3 = new Web3(Web3.givenProvider || "etherium network address:port");

// 지갑 개수만큼 local에 지갑 생성
wallet = web3.eth.accounts.wallet.create("지갑 개수")

// privateKey, walletAddress 추출
this.privateKey = wallet[0].privateKey;
this.walletAddress = wallet[0].address;
```

- front-end에서 web3js api를 사용해 etherium network에 지갑 생성
- back-end 서버에 walletAddress 저장

<br>

### 지갑을 통한 코인 충전

- 계약 계정에서 외부 소유 계정으로 코인을 송금

<br>

> web3.j를 사용한 충전

```java
try {
    // 1. CA를 unlock
    PersonalUnlockAccount personalUnlockAccount = admin.personalUnlockAccount(
        "CA address", "CA password").send();

    if(personalUnlockAccount.accountUnlocked()) {
        // nonce를 초기화 하기 위해 CA의 transaction 수를 불러온다
        EthGetTransactionCount ethGetTransactionCount = web3j.ethGetTransactionCount(
            "CA address", DefaultBlockParameterName.LATEST).send();
		
        // CA의 transaction수를 통해 nonce를 초기화
        BigInteger nonce = ethGetTransactionCount.getTransactionCount();
        BigInteger gasPrice = BigInteger.valueOf(5);
        BigInteger gasLimit = BigInteger.valueOf(100000);
        BigInteger value = BigInteger.valueOf(5);
		
        // 2. transaction 생성
        Transaction transaction = Transaction.createEtherTransaction(
            "CA address",	// from
            nonce,			// nonce
            gasPrice,		// gasPrice
            gasLimit,		// gasLimit
            주소,			   // to
            value);			// 송금 코인(eth)
		
        // 3. transaction을 보낸 후 결과를 수신
        EthSendTransaction ethSendTransaction = web3j.ethSendTransaction(transaction).send();
		
        // 4. transaction의 hash값을 저장+
        String transactionHash = ethSendTransaction.getTransactionHash();

        return transactionHash;
    }

} catch (Exception e) {
    e.printStackTrace();
}
```

- web3j로 web3j 객체를 생성하고, 트랜잭션을 생성하여 요청
- nonce: transaction의 수 이며 unique해야 하는 값
  - unique하지 않아도 transaction이 생성은 되나 보내지는 것은 하나만 보내진다.
  - unique한 값을 갖기 위해 생성된 transaction의 수를 통해 nonce를 초기화

- 적절한 gasPrice와 gasLimit을 정하는 방법
  - 너무 낮거나 높은 값들을 설정하면 transaction이 송신되지 않는다.



