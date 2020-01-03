---
title: "매일프로그래밍 알고리즘, 모든 단어를 거꾸로 출력"
excerpt: "본 글은 '매일프로그래밍'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 저작권은 '매일프로그래밍'에게 있습니다. 문제는 다음과 같습니다. Reverse all the words in the given string."
search: true
categories: 
  - Algorithm
  - Mailprogramming
tags: 
  - 알고리즘
  - Python
  - 파이썬
  - 매일프로그래밍
last_modified_at: 2019-04-29T21:04:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190429-mailprogramming-reverse-string.png
---

<i class="fas fa-exclamation-circle"></i> 아래 문제는 **매일 프로그래밍**에서 가져온 문제입니다. **문제의 저작권은 매일프로그래밍에게 있습니다.** 해당 사이트에서 문제를 접할 수 있습니다. <a href="https://mailprogramming.com/" target="_blank">https://mailprogramming.com/</a>
{: .notice--danger}

## 문제

주어진 `string` 에 모든 단어를 거꾸로 하시오.  

Reverse all the words in the given string. 


## 예제

```javascript
Input: "abc 123 apple"
Output: "cba 321 elppa"
```

## 문제풀이

각 단어를 거꾸로 만들어야 하므로, 우선은 단어별로 나눠야합니다. 그 다음에 각각의 단어를 역순으로 바꿔주면 됩니다. 마지막으로 완성된 문자열을 하나의 문자열로 결합합니다.   

- 입력받은 문자열을 단어별로 나눈다.
- 나눈 단어를 각각 역순으로 바꾼다.
- 역순으로 바꾼 단어를 공백(스페이스)을 두고 하나의 문자열로 합친다.

### 1. Python으로 풀기

파이썬으로 풀게되면 정말 간단하게 풀 수 있습니다. 파이썬에는 `map()` 함수가 내장 함수로 존재하기 때문에 따로 구현할 필요가 없습니다.  

(사실 `map()`함수를 구현한다고해도 쉽게 만들 수 있음)

```python
# -*-coding: utf-8 -*-
import sys

def solution(string):
    return " ".join(map(lambda x: x[::-1], string.split()))

if __name__ == "__main__":
    _input = sys.stdin.readline()

    print(solution(_input))
```

```bash
# "abc 123 apple"  # Input
cba 321 elppa  # Output

# hello python 123456  # Input
olleh nohtyp 654321  # Output
```

#### 코드 설명

천천히 코드를 설명하면, `.split()`은 문자열을 구분자로 나눠서 리스트로 만들어줍니다. 인자를 받지 않으면, 공백(`default`)으로 나눕니다. 예를들어,  

```python
str_list = string.split()  # string = "hello python 123"
print(str_list)  # ["hello", "python", "123"]
```

두번째, `lambda x: x[::-1]`는 람다식(익명 함수)과 리스트 인덱싱을 이용한 것으로, 역순으로 바꿔주는 역할을 합니다. 예를들어,  

```python
def reverse(string):
  return string[::-1]

print(reverse("hello"))  # olleh
```

세번째, `map()`은 인자를 2개를 받아서 왼쪽의 리스트 원소를 각각 오른쪽 함수에 적용을(`mapping`) 시킵니다. 예를들어,  

```python
print(list(map(lambda x: x[0], ["hello", "python", "123"])))
# 결과: ["h", "p", "1"]
```

마지막으로 `.join()`은 리스트를 앞의 문자열을 구분자로하여 하나의 문자열로 결합해줍니다. 예를들어,  

```python
print("/".join(["hello", "python", "123"]))
# 결과: hello/python/123
```


<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}