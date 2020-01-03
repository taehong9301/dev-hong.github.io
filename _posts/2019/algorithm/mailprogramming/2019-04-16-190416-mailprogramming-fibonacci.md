---
title: "매일프로그래밍 알고리즘, 피보나치 수열의 합"
excerpt: "본 글은 '매일프로그래밍'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 저작권은 '매일프로그래밍'에게 있습니다. 문제는 다음과 같습니다. Fibonacci sequence starts with 0 and 1 where each fibonacci number is a sum of two previous fibonacci numbers. Given an integer N, find the sum of all even fibonacci numbers."
search: true
categories: 
  - Algorithm
  - Mailprogramming
tags: 
  - 알고리즘
  - Python
  - 파이썬
  - 매일프로그래밍
last_modified_at: 2019-04-16T19:54:48+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190416-mailprogramming-fibonacci-500x500.png
---

<i class="fas fa-exclamation-circle"></i> 아래 문제는 **매일 프로그래밍**에서 가져온 문제입니다. **문제의 저작권은 매일프로그래밍에게 있습니다.** 해당 사이트에서 문제를 접할 수 있습니다. <a href="https://mailprogramming.com/" target="_blank">https://mailprogramming.com/</a>
{: .notice--danger}

## 문제

피보나치 배열은 0과 1로 시작하며, 다음 피보나치 수는 바로 앞의 두 피보나치 수의 합이 된다. 정수 N이 주어지면, N보다 작은 모든 짝수 피보나치 수의 합을 구하여라.  

Fibonacci sequence starts with 0 and 1 where each fibonacci number is a sum of two previous fibonacci numbers. Given an integer N, find the sum of all even fibonacci numbers.  


<i class="fas fa-feather-alt"></i> **피보나치 수열이란?** 피보나치 수열 `Fibonacci numbers`은 첫째항과 둘째항이 1을 가지고, 그 뒤의 항부터는 앞의 2개의 항의 합인 수열입니다. 그래서, 피보나치 수열은 다음과 같은 수열을 갖습니다. `1, 1, 2, 3, 5, 8, 13, 21, 34 ...` 편의상 0번째 항을 0으로 두기도 합니다. 위의 문제는 첫 항을 0으로 둔 피보나치 수열을 이용하여 문제를 접근하고 있습니다.
{: .notice--info}

## 예제

```javascript
Input: 12
Output: 10 // 0, 1, 2, 3, 5, 8 중 짝수인 2 + 8 = 10.
```

## 문제 풀이

이 문제의 핵심은 **반복 연산을 피하는 것**이라고 생각합니다. 숫자가 작은 경우에는 피보나치 수열의 연산 과정이 적습니다. 하지만 **숫자가 커질수록 연산과정이 기하급수적으로 증가**하게 됩니다. 따라서, **메모이제이션** `memoization` 방법을 이용하는 것이 중요합니다.  

<i class="fas fa-feather-alt"></i> **메모이제이션이란?** 메모이제이션은 이전에 계산한 값에 대해서는 다시 계산하지 않도록 메모리에 값을 저장하는 방법을 말합니다.
{: .notice--info}

단순하게 메모이제이션을 사용하지 않고 문제를 풀어보면, (해당 예제는 파이썬으로 작성되었습니다.)

```python
def fibo(n):
    if n < 2:
        return n
    return fibo(n-1) + fibo(n-2)

def solve(n):
    result = 0
    for n in range(2, n):
        if fibo(n) % 2 == 0:
            result += fibo(n)
    return result
```

위의 코드는 숫자 `n`이 30이상만 되더라도 연산하는 과정이 기하급수적으로 많아져서 값이 나오지 않게 됩니다.


| 회차 | n | 계산식 |
| --- | --- | --- |
| 1회 | 0 | `0` |
| 2회 | 1 | `1` |
| 3회 | 2 | `0` + `1` |
| 4회 | 3 | `(0 + 1)` + `(1 + (0 + 1))` |
| 5회 | 4 | `(1 + (0 + 1))` + `((0 + 1) + (1 + (0 + 1)))` |
| 6회 | 5 | `((0 + 1) + (1 + (0 + 1)))` + `((1 + (0 + 1)) + (0 + 1) + (1 + (0 + 1)))` |

그 뒤의 계산식을 적지 않더라도 연산량이 매우 많아지는 것을 알 수 있습니다. 하지만, 잘보면 **이미 계산한 연산을 반복해서 연산**을 하고 있습니다. 위에서 살펴봤듯이 이런 경우에는 **메모이제이션을 사용**하는 것이 좋습니다.  

메모이제이션을 이용하여 문제를 풀어봅시다.

## 1. Python으로 풀기
```python
# -*-coding: utf-8 -*-
from collections import defaultdict

def solution(n):
    """memoization을 이용하여 시간 단축

    variable:
        result: 반환값(피보나치 수열에서 n보다 작은 짝수들의 합)
        memo: 연산된 값을 저장하기 위한 Dictionary 자료형
    """
    result = 0
    memo = defaultdict(lambda: -1)  # defaultdict을 이용. 기본값 -1
    memo[0] = 0
    memo[1] = 1
        
    for k in range(n):
        if memo[k] != -1:
            continue  # 이미 계산한적이 있음
        else:
            memo[k] = memo[k-1] + memo[k-2]
            if memo[k] < n:
                # n 보다 작은 경우만
                if memo[k] % 2 == 0:
                    result += memo[k]
            else:
                break  # n보다 크거나 같은 경우 종료
    return result


if __name__ == "__main__":
    test_case = [12, 144, 300, 800, 1000]
    
    for test in test_case:
        print("Input:", test, "| Output:", solution(test))
```

```python
Input: 12 | Output: 10
Input: 144 | Output: 44
Input: 300 | Output: 188
Input: 800 | Output: 798
Input: 1000 | Output: 798
```

메모이제이션을 사용하여 풀었기 때문에, 1000이라는 **큰 숫자를 넣어도 1초도 안걸리고 순식간에 값이 나오는 것**을 확인 할 수 있습니다. 반면, 메모이제이션을 사용하지 않는 경우 30만 넘어도 값이 나오지 않고 멈추는 현상을 볼 수 있습니다.  

위에서는 메모이제이션 자료형을 파이썬의 딕셔너리를 이용하여 풀었지만, 배열로 풀어도 상관없습니다.  

<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}


## Reference

[https://ko.wikipedia.org/wiki/레오나르도_피보나치](https://ko.wikipedia.org/wiki/%ED%94%BC%EB%B3%B4%EB%82%98%EC%B9%98_%EC%88%98 "피보나치 수열 WIKI")  

[https://ko.wikipedia.org/wiki/메모이제이션](https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98 "메모이제이션 WIKI")  