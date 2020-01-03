---
title: "백준 1000번 A+B"
excerpt: "본 글은 '백준'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 출처는 'baekjoon'입니다. 문제는 다음과 같습니다. 두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성하시오."
search: true
categories:
  - Algorithm
  - Baekjoon
tags:
  - 알고리즘
  - Python
  - 백준
last_modified_at: 2019-04-27T00:41:00+09:00
toc: true
toc_sticky: true
comments: true
# header:
#     teaser: /assets/images/thumbnail/2019/190427-baekjoon-algorithm-ex1000.png
---

<i class="fas fa-exclamation-circle"></i> 아래 문제는 **백준**에서 가져온 문제입니다. 해당 사이트에서 문제를 접할 수 있습니다. <a href="https://www.acmicpc.net/problem/1000" target="_blank">https://www.acmicpc.net/problem/1000</a>
{: .notice--danger}

## 문제

두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성하시오.

## 예제

```bash
# 입력
1 2

# 출력
3
```

## 풀이

문제는 굉장히 쉽습니다. 단순히 2개의 수를 입력을 받아 두 수의 합을 구하는 문제입니다.

### 1. Python으로 풀기

파이썬에는 콘솔입력을 받는 방법이 2가지가 있습니다.

- input()
- sys.stdin.readline()

속도면에서는 두번째 `sys`모듈의 `.stdin.readline()`을 사용하는 것이 더 빠릅니다. 그렇기 때문에 알고리즘 문제를 풀때는 이 방법을 사용하는 것이 좋습니다.

```python
import sys
_ = map(int, sys.stdin.readline().split())
print(sum(_))
```

문제는 굉장히 쉽지만, 파이썬을 처음 접하는 분에게는 위 코드도 어려울 수 있습니다. 여기서 알아야 할 것은 3가지입니다.

- `sys.stdin.readline()`: **표준 콘솔 입력** 방법으로 한줄을 입력받습니다.
- `.split()`: split() 메소드 안에 구분자를 넣어주게 되면, 구분자를 기준으로 **문자열을 나눠 리스트로 반환**합니다. 구분자를 넣어주지 않으면 공백을 구분자로 갖습니다.
- `map()`: map() 함수를 이용하여 **매핑**이 가능합니다. 위의 경우로 예를 들면, 왼쪽의 리스트를 `int` 정수로 파싱 `parsing` 합니다.

## 결과

| 채점 번호 | 아이디 | 문제 번호 | 결과         | 메모리  | 시간 | 언어     | 코드 길이 |
| :-------- | :----- | :-------- | :----------- | :------ | :--- | :------- | :-------- |
| 12934481  | hongku | 1000      | 맞았습니다!! | 29056KB | 60ms | Python 3 | 67B       |

<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}
