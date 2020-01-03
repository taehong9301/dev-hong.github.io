---
title: "매일프로그래밍 알고리즘, 괄호 조합"
excerpt: "본 글은 '매일프로그래밍'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 저작권은 '매일프로그래밍'에게 있습니다. 문제는 다음과 같습니다. Given an integer N, find the number of possible balanced parentheses with N opening and closing brackets."
search: true
categories: 
  - Algorithm
  - Mailprogramming
tags: 
  - 알고리즘
  - Python
  - 파이썬
  - 매일프로그래밍
last_modified_at: 2019-04-23T22:50:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190423-mailprogramming-brackets-560x315.png
---

<i class="fas fa-exclamation-circle"></i> 아래 문제는 **매일 프로그래밍**에서 가져온 문제입니다. **문제의 저작권은 매일프로그래밍에게 있습니다.** 해당 사이트에서 문제를 접할 수 있습니다. <a href="https://mailprogramming.com/" target="_blank">https://mailprogramming.com/</a>
{: .notice--danger}

## 문제

정수 `n`이 주어지면, `n`개의 여는 괄호 `"("`와 `n`개의 닫는 괄호 `")"`로 만들 수 있는 괄호 조합을 모두 구하시오. (시간 복잡도 제한 없습니다).  

Given an integer N, find the number of possible balanced parentheses with N opening and closing brackets.  


## 예제

```javascript
Input: 1
Output: ["()"]
```

```javascript
Input: 2
Output: ["(())", "()()"]
```

```javascript
Input: 3
Output: ["((()))", "(()())", "()(())", "(())()", "()()()"]
```


## 문제풀이

재귀함수를 이용하여 풀면 좋습니다. 핵심은 괄호는 쌍으로 이루어져있기 때문에 열리면 다시 닫아줘야 합니다. 즉, 괄호를 열었으면 닫아줘야하고, 괄호를 `open`한 횟수, `close`한 횟수가 각각 같아야 합니다.  

1. 재귀함수를 이용한다.
2. 괄호를 열었으면 닫아준다.

### 1. Python으로 풀기

```python
# -*- coding: utf-8 -*-


def solution(n, _open, close, bracket, result):
    if close == n:
        result.append(bracket)  # 닫는 괄호까지 모두 마쳤다면, 리스트에 추가
        return

    # 열었으면 닫아준다
    # 열고
    if _open < n:
        solution(n, _open + 1, close, bracket + "(", result)

    # 닫고
    if close < _open:
        solution(n, _open, close + 1, bracket + ")", result)


# main
if __name__ == "__main__":
    test_case = [1, 2, 3, 4, 5]

    for test in test_case:
        result = list()
        solution(test, 0, 0, "", result)
        print("Input:", test, "| Output:", result)
```

```python
Input: 1 | Output: ['()']
Input: 2 | Output: ['(())', '()()']
Input: 3 | Output: ['((()))', '()(())', '(())()', '(()())', '()()()']
Input: 4 | Output: ['(((())))', '()((()))', '((()))()', '(()(()))', '()()(())', '()(())()', '((())())', '()(())()', '(())()()', '((()()))', '()(()())', '(()())()', '(()()())', '()()()()']
```

글쓴이 같은 경우는 처음에 `for`문을 이용하여 풀었다가 놓치는 부분이 많았습니다. (즉, 잘못 풀고 있었습니다.)  

`n`이 증가하면 3가지 경우만 추가되는줄 알았습니다.  

- 첫번째, 앞뒤로 괄호가 붙는 경우 `"(" + 이전값 + ")"`
- 두번째, 앞에 괄호가 붙는 경우 `"()" + 이전값`
- 세번째, 뒤에 괄호가 붙는 경우 `이전값 + "()"`

하지만 다른 경우도 있더군요. 예를들어서 `n = 5`인 경우에서, `((())(()))` 이와 같은 경우는 위 3가지 조건으로 만들어 질 수 없습니다. 반대로 따라가보면...  

|n|brackets|
|:---|:---|
|5|`((())(()))`|
|4|`(())(())`|
|3|`())(()`|

안되는것이 보이시나요?  

## 결론

`n=4`일 때까지만 풀어보다가 답이 똑같이 나와서 맞은 줄 알고, 매일프로그래밍 해답을 봤습니다. 하지만 `n=5`일 때 부터 값이 이상하더군요. 틀렸는데 맞은줄 알고 있었네요..  

결국 매일프로그래밍에서 얻은 해답으로 풀긴했습니다만, 아직 알고리즘 공부가 부족하다는 걸 다시 한번 느꼈네요. 더 열심히 해야할 것 같습니다. 아무튼 다들 열심히 공부하시길 바랍니다:)

<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}