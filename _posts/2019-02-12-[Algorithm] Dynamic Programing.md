---
Title: Dynamic Programming
tags: ["Algorithm", "DP"]
---



# Dynamic Programming

##### 2019. 02. 12 (Tue)

<br>

입력 크기가 작은 부분을 모두 해결하고, **그 해를 이용하여 더 큰 크기의 문제를 해결**한다.

- **이미 해결한 문제는 다시 계산하지 않는다.**

결국 작은 부분과 더 큰 부분사이 관계를 구하여 문제를 해결하는 방법

<br>

<br>

## I. fibonacci numbers

### 1. 재귀적 완전탐색

```c++
int fibonacci_rec(int n) {
	if(n == 0) return 0;
    if(n == 1) return 1;
    
    return fibonacci_rec(n-1) + fibonacci_rec(n-2);
}
```

구현이 간단

**재귀**로 수행하며, **중복 계산이 발생**

finonacci_rec(5) 를 수행할 경우 n = 1, 2, 3, 4에 대해 이미 계산 되었어도 다시 계산하는 일이 발생

<br>

#### 중복을 발생시키지 않는 방법?

한번 계산된 결과를 **table에 기록**한다.
- 입력값이 n개: n차 배열에 저장
- fibonacci의 경우 입력값이 n 한개 이므로 n크기의 1차 배열에 저장한다.

이미 **계산된 결과가 존재**할 경우 table에서 **계산 결과를 찾아서 사용**

<br>

<br>

### 2. memoization

```c++
//-1로 초기화된 n개 크기의 1차 배열
int memo[n];

int fibonacci_memo(int n) {
    if(n == 0) return 0;
    if(n == 1) return 1;
    
	if(memo[n] == -1) return memo[n] = fibonacci_memo(n-1) + fibonacci_memo(n-2);
    
    return memo[n];
}
```

n에 대해 계산 결과가 없다면, 계산을 하여 **기록**하고 반환

계산 결과가 있다면, 계산 결과를 반환

그러나 여전히 재귀적으로 작동하므로 **반복문**으로 처리하여 더 빠르게 계산

<br>

<br>

### 3. Dynamic Programming

```c++
int fibonacci_dp(int n) {
	// -1로 초기화된 n개 크기의 1차배열
    int dp[n];

    // 점화식 시작을 위한 초기화
    dp[0] = 0;
    dp[1] = 1;

    for(int i = 2; i < n+1; ++i) {
        // fibonacci numbers의 관계는 f(n) = f(n-1) + f(n-2) 관계
        // 작은 문제 부터 큰 문제까지 올라간다.
        dp[i] = dp[i-1] + dp[i-2];
    }
    
    return dp[n];
}
```

#### 구현 방법

1. 문제를 **작은 부분**으로 나눈다.
2. 큰 문제와 작은 문제간 **관계**를 구하고, 작은 문제를 계산
   - 관계를 표현 : 분석, design문제.
     1. f(n)이 무엇이다 정의
     2. f(n)과 f(n-1), f(n-2)...와의 관계 정의
3. 큰 문제를 해결

<br>

작은문제를 계산하여 큰 문제를 해결
- 작은 문제: f(n-1), f(n-2)...
- 큰 문제: f(n)

문제간 관계(점화식)을 표현

- 점화식: f(n) = f(n-2) + f(n-1)

작은 문제에서 올라가게끔 반복구조로 작성

<br>

<br>

### (4) 결과

fibonacci_rec(): 작성하기는 쉬우나, 중복이 심하고 수행 시간이 오래걸린다.

fibonacci_memo(): 작성하기 쉽고, 재귀버전이므로 더욱 빠르게 반복으로 변환한다.

fibonacci_dp(): 관계를 표현하기 어렵지만, 반복문을 활용하여 더욱 빠르게 수행한다.