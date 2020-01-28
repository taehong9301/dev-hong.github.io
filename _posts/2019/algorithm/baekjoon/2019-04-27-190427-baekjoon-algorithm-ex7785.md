---
title: "백준 7785번 회사에 있는 사람"
excerpt: "본 글은 '백준'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 출처는 'baekjoon'입니다. 문제는 회사에 입출입하는 사람들 중 마지막에 남은 회사원을 골라내는 문제입니다."
search: true
categories:
  - Algorithm
  - Baekjoon
tags:
  - 알고리즘
  - Python
  - 백준
last_modified_at: 2019-04-27T10:28:00+09:00
toc: true
toc_sticky: true
comments: true
header:
  teaser: https://user-images.githubusercontent.com/26136312/73272566-ee73a180-4225-11ea-953c-88d40b8577a4.png
---

<i class="fas fa-exclamation-circle"></i> 아래 문제는 **백준**에서 가져온 문제입니다. 해당 사이트에서 문제를 접할 수 있습니다. <a href="https://www.acmicpc.net/problem/7785" target="_blank">https://www.acmicpc.net/problem/7785</a>
{: .notice--danger}

## 문제

상근이는 세계적인 소프트웨어 회사 기글에서 일한다. 이 회사의 가장 큰 특징은 자유로운 출퇴근 시간이다. 따라서, 직원들은 반드시 9시부터 6시까지 회사에 있지 않아도 된다.

각 직원은 자기가 원할 때 출근할 수 있고, 아무때나 퇴근할 수 있다.

상근이는 모든 사람의 출입카드 시스템의 로그를 가지고 있다. 이 로그는 어떤 사람이 회사에 들어왔는지, 나갔는지가 기록되어져 있다. 로그가 주어졌을 때, 현재 회사에 있는 모든 사람을 구하는 프로그램을 작성하시오.

## 예제

```bash
# 입력
4
Baha enter
Askar enter
Baha leave
Artem enter

# 출력
Askar
Artem
```

## 풀이

문제는 쉽습니다. 반복문을 돌리면서 사람들에 대한 상태(출근 또는 퇴근)를 저장하고 있다가 마지막에 출근한 사람만을 출력하면 됩니다.

사람들의 이름과 그 사람의 상태를 저장하고 있어야 하기 때문에, 맵(`map`)형태의 자료구조를 이용하면 편합니다.

### 1. Python으로 풀기

- `Dictionary`를 이용하여 데이터를 저장
- 속도 향상을 위해, 콘솔 입력을 `sys.stdin.readline()` 사용
- 메모리 최소화 및 속도향상을 위해 `leave`(퇴근)인 경우 `del`로 메모리 해제

#### 1.1. 코드

```python
# -*-coding: utf-8 -*-

import sys
n = int(sys.stdin.readline())

result = dict()
for _ in range(n):
    name, action = sys.stdin.readline().split()
    if action == "enter":
        result[name] = True
    else:
        del result[name]

print("\n".join(sorted(result.keys(), reverse=True)))
```

#### 1.2. 부가설명 및 다른 방법

사실 메모리를 따로 해제하지 않더라도 `reference count`가 `0`이되면, 파이썬의 `GC`가 알아서 메모리를 해제해줍니다. 하지만 위 문제에서 강제로 메모리 해제해주는 이유는 메모리 최소화 하는 이유도 있지만, 속도 향상을 위한것도 있습니다.

사람들의 상태를 모두 구한 뒤에, 아래와 같이 역 정렬 후 출력을 하는데

```python
# 채점결과: 메모리: 40580KB / 시간: 164ms
print("\n".join(sorted(result.keys(), reverse=True)))
```

만약 `del`을 이용하여 메모리 해제를 안한 경우...

```python
# 채점결과: 메모리: 47624KB / 시간: 240ms
for name, action in sorted(result.items(), reverse=True)):
    if action == "enter":
        print(name)
```

위 처럼 정렬을 할때, `.items()`에 의해 `tuple` 형태의 자료구조를 다뤄야해서 더 시간이 소모가 됩니다.

그래도 위 방법이 좋다면, 아래와 같이 하면 시간을 더 단축시킬 수 있습니다.

```python
# 채점결과: 메모리: 40580KB / 시간: 216ms
for name in sorted(result.keys(), reverse=True)):
    if result[name] == "enter":
        print(name)
```

## 결과

| 채점 번호 | 아이디 | 문제 번호 | 결과         | 메모리  | 시간  | 언어     | 코드 길이 |
| :-------- | :----- | :-------- | :----------- | :------ | :---- | :------- | :-------- |
| 12934481  | hongku | 7785      | 맞았습니다!! | 40580KB | 164ms | Python 3 | 293B      |

<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}
