---
title: "매일프로그래밍 알고리즘, 암호화 가능 여부"
excerpt: "본 글은 '매일프로그래밍'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 저작권은 '매일프로그래밍'에게 있습니다. 문제는 다음과 같습니다. Given two strings of equal length, check if two strings can be encrypted 1 to 1."
search: true
categories:
  - Algorithm
  - Mailprogramming
tags:
  - 알고리즘
  - Python
  - 파이썬
  - 매일프로그래밍
last_modified_at: 2019-06-12T19:30:00+09:00
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

길이가 같은 두 문자열(`string`) `A` 와 `B`가 주어지면, `A` 가 `B` 로 1:1 암호화 가능한지 찾으시오.

Given two strings of equal length, check if two strings can be encrypted 1 to 1.

## 예제

```javascript
Input: "EGG", "FOO";
Output: True; // E->F, G->O
```

```javascript
Input: "ABBCD", "APPLE";
Output: True; // A->A, B->P, C->L, D->E
```

```javascript
Input: "AAB", "FOO";
Output: False;
```

## 문제 풀이

키/값 형태를 갖는 맵(`map`) 자료형을 이용하여 문제를 풀었습니다.

맵을 이용하여 데이터를 저장하는데, 그 데이터는 평문(`plain`) 문자를 키 값으로 가지고, 암호화(`encrypt`) 문자를 값으로 갖습니다.

- `key`: 평문 문자
- `value`: 암호화 문자

이때 나타날 수 있는 상황은 2가지가 있습니다.

1. 처음으로 저장된 문자인 경우(키 값이 존재하지 않는 경우): 맵 자료형에 데이터(키: 평문문자 / 값: 암호화 문자)를 저장합니다.

2. 이미 저장된 문자인 경우(키 값이 존재하는 경우): 맵 자료형에서 이전에 저장된 값을 가져와 비교를 합니다.

- 만약 같은 경우 이어서 반복문을 수행
- 다른 경우는 1:1 암호화가 안되는 경우이기 때문에, `False`를 반환하고 프로그램을 종료합니다.

### Python으로 풀기

**참고** 해답을 확인하지 않았기 때문에 매일프로그래밍에서 주어진 해답과 문제 풀이가 다를 수 있습니다. 참고바랍니다.
{: .notice--warning}

```python
# -*- coding: utf-8 -*-

import sys


def solve(plain, encrypt):
  # 유효성 검사
  assert len(plain) == len(encrypt), "Invalid value for argument. <{}, {}>".format(plain, encrypt)

  # 비교 시작
  alpa_dict = dict()  # 평문 문자에 대한 암호화 문자를 담고 있는 맵 자료형
  for i in range(len(plain)):
    p_c = plain[i]  # 평문 문자
    e_c = encrypt[i]  # 암호화 문자

    if p_c in alpa_dict.keys():
      # Case1. 이미 나온 문자
      if alpa_dict[p_c] != e_c:
        return False  # 이미 등록된 문자와 다른 경우, 거짓
    else:
      # Case2. 처음 나온 문자
      alpa_dict[p_c] = e_c

  return True


if __name__ == "__main__":
  _input = sys.stdin.readline().split(",")
  plain = _input[0].strip()
  encrypt = _input[1].strip()

  print(solve(plain, encrypt))

```

```python
# Input
EGG, FOO

# Output
True
```

```python
# Input
ABBCD, APPLE

# Output
True
```

```python
# Input
AAB, FOO

# Output
False
```

<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}
