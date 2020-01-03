---
title: "매일프로그래밍 알고리즘, 가장 긴 접두사"
excerpt: "본 글은 '매일프로그래밍'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 저작권은 '매일프로그래밍'에게 있습니다. 문제는 다음과 같습니다. Given an array of strings, find the longest common prefix of all strings."
search: true
categories: 
  - Algorithm
  - Mailprogramming
tags: 
  - 알고리즘
  - Python
  - 파이썬
  - 매일프로그래밍
last_modified_at: 2019-04-21T20:51:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190421-mailprogramming-longest-prefix-560x315.png
---

<i class="fas fa-exclamation-circle"></i> 아래 문제는 **매일 프로그래밍**에서 가져온 문제입니다. **문제의 저작권은 매일프로그래밍에게 있습니다.** 해당 사이트에서 문제를 접할 수 있습니다. <a href="https://mailprogramming.com/" target="_blank">https://mailprogramming.com/</a>
{: .notice--danger}

## 문제

문자열 배열(string array)이 주어지면, 제일 긴 공통된 접두사(prefix)의 길이를 찾으시오.  

Given an array of strings, find the longest common prefix of all strings.  

## 예제

```javascript
Input: ["apple", "apps", "ape"]
Output: 2 // "ap"
```

```javascript
Input: ["hawaii", "happy"]
Output: 2 // "ha"
```

```javascript
Input: ["dog", "dogs", "doge"]
Output: 3 // "dog"
```

## 문제풀이

문제를 풀기 위한 핵심(?)은 2가지였습니다.  

첫번째, 각 원소의 한 글자씩를 비교해야합니다. 즉, **같은 인덱스의 문자를 비교**를 해야합니다. 그렇기 때문에 루프문을 돌릴 때 원소의 인덱스로 돌려야 합니다. 예를 들어보면 아래 표와 같이 할 수 있습니다. `["apple", "apps", "ape"]` 문자열 배열이 있을때,  

|Index |루프 횟수|apple|apps|ape|결과|
|:---|:---|:---|:---|:---|:---|
| 1 | 1회| `a` | `a` | - | `pass` |
| 1 | 2회| - | `a` | `a` | `0` + 1 = `1` |
| 2 | 1회| `p` | `p` | - | `pass` |
| 2 | 2회| - | `p` | `p` | `1` + 1 = `2` |
| 3 | 1회| `p` | `p` | - | `pass` |
| 3 | 2회| - | `p` | `e` | `return 2` |


두번째, 첫번째 원소를 건너뛰고 두번째 원소부터 시작해야 합니다. 왜냐하면 **이전값과 현재값을 비교**하기 위해서입니다. (파이썬) 코드로 설명하면 `for i in range(1, len(arr))`, 반대로 마지막 원소를 제외하고 루프를 돌리는 방법도 있습니다. `for i in range(len(arr)-1)`와 같이 할 수 있습니다. 어떤 방법을 하든 선택입니다.   

### 1. Python으로 풀기

```python
# -*- coding: utf-8 -*-

def solution(arr):
    result = 0

    for i in range(len(arr[0])):
        for j in range(1, len(arr)):
            if len(arr[j]) < i + 1 or arr[j-1][i] != arr[j][i]:
                return result  # 인덱스를 벗어나는 경우 또는 전의 값과 지금 값이 다른경우(결과 반환)
        result += 1

    return result


if __name__ == "__main__":
    test_case = [
        ["apple", "apps", "ape"],
        ["hawaii", "happy"],
        ["dog", "dogs", "doge"],
        ["banana", "b", "banana"],
        ["happy", "happy", "happy"]
    ]

    for test in test_case:
        print("Input:", test, "| Output:", solution(test))
```

```python
Input: ['apple', 'apps', 'ape'] | Output: 2
Input: ['hawaii', 'happy'] | Output: 2
Input: ['dog', 'dogs', 'doge'] | Output: 3
Input: ['banana', 'b', 'banana'] | Output: 1
Input: ['happy', 'happy', 'happy'] | Output: 5
```

처음에는 문자열을 구하는건줄 알고 문자열을 따로 저장했었지만, 문제를 다시보니 접두사 길이를 구하는 문제였습니다. 즉, 문자를 따로 저장할 필요가 없더군요..  

시간복잡도는 `O(nm)`, 공간복잡도는 `O(1)`로 풀긴 했지만, 파이썬 답지 않은 풀이 같습니다. 좀 더 파이썬 답게 풀 수 있을거 같은데, 생각이 안나네요. 다른 분들은 어떻게 풀었을지 궁금합니다.  

## Reference

<a href="https://ko.wikipedia.org/wiki/%ED%95%B4%EC%8B%9C_%ED%85%8C%EC%9D%B4%EB%B8%94" target="_blank">https://ko.wikipedia.org/wiki/해시_테이블</a>  


<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}