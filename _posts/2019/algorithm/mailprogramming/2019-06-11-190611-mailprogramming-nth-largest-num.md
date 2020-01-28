---
title: "매일프로그래밍 알고리즘, N번째 큰 수"
excerpt: "본 글은 '매일프로그래밍'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 저작권은 '매일프로그래밍'에게 있습니다. 문제는 다음과 같습니다. Given an integer array and integer N, find the Nth largest element in the array."
search: true
categories:
  - Algorithm
  - Mailprogramming
tags:
  - 알고리즘
  - Python
  - 파이썬
  - 매일프로그래밍
last_modified_at: 2019-06-11T22:05:00+09:00
toc: true
toc_sticky: true
comments: true
header:
  teaser: https://user-images.githubusercontent.com/26136312/73272717-2ed31f80-4226-11ea-9563-9ea087f62798.png
exam-gallery:
  - url:
    image_path:
    alt: ""
    title: ""
---

<i class="fas fa-exclamation-circle"></i> 아래 문제는 **매일 프로그래밍**에서 가져온 문제입니다. **문제의 저작권은 매일프로그래밍에게 있습니다.** 해당 사이트에서 문제를 접할 수 있습니다. <a href="https://mailprogramming.com/" target="_blank">https://mailprogramming.com/</a>
{: .notice--danger}

## 문제

정수 배열(`int array`)과 정수 `N`이 주어지면, `N`번째로 큰 배열 원소를 찾으시오.

Given an integer array and integer N, find the Nth largest element in the array.

## 예제

```python
Input: [-1, 3, -1, 5, 4], 2
Output: 4
```

```python
Input: [2, 4, -2, -3, 8], 1
Output: 8
```

```python
Input: [-5, -3, 1], 3
Output: -5
```

## 문제 풀이

사실 문제는 파이썬(`Python`)으로 풀게되면 굉장히 쉽게 풀 수 있는 문제입니다. 거기다 시간복잡도, 공간복잡도에 대한 내용 또한 없기 때문에 `sorted()` 함수를 이용하면 됩니다.

만약 `C`와 같은 언어나 내장 함수를 사용하지 않고 푼다면, 정렬(`sorting`)을 이용하여 풀면 될 것 같습니다. (사실 내장함수를 사용하기보단 정렬 공부도 할겸 삽입 정렬, 버블 정렬, 퀵 정렬 등등을 이용하는 방법이 좋을 것 같습니다.)

### Python으로 풀기

**참고** 해답을 확인하지 않았기 때문에 매일프로그래밍에서 주어진 해답과 문제 풀이가 다를 수 있습니다. 참고바랍니다.
{: .notice--warning}

```python
# -*- coding: utf-8 -*-

import sys


def solve(arr, n):
  rtn = sorted(arr, reverse=True)  # reverse 옵션이 없으면 default 값은 False
  return rtn[n-1]


if __name__ == "__main__":
  arr = list(map(int, sys.stdin.readline().split()))
  n = int(sys.stdin.readline())

  print(solve(arr, n))
```

```python
Input
-1 3 -1 5 4
2

Output
4
```

```python
Input
2 4 -2 -3 8
1

Output
8
```

```python
Input
-5 -3 1
3

Output
-5
```

<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}
