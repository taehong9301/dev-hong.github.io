---
title: "매일프로그래밍 알고리즘, 0을 오른쪽으로 옮기기"
excerpt: "본 글은 '매일프로그래밍'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 저작권은 '매일프로그래밍'에게 있습니다. 문제는 다음과 같습니다. 정수 배열(int array)이 주어지면 0이 아닌 정수 순서를 유지하며 모든 0을 배열 오른쪽 끝으로 옮기시오."
search: true
categories: 
  - Algorithm
  - Mailprogramming
tags: 
  - 알고리즘
  - Python
  - 파이썬
  - 매일프로그래밍
last_modified_at: 2019-05-15T23:05:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190515-mailprogramming-move-zero-right.png
exam-gallery:
  - url: https://user-images.githubusercontent.com/26136312/57781783-d62a2800-7765-11e9-9182-5dfff851b0da.png
    image_path: https://user-images.githubusercontent.com/26136312/57781783-d62a2800-7765-11e9-9182-5dfff851b0da.png
    alt: "예제 그림"
    title: "일반 리스트"
exam-gallery2:
  - url: https://user-images.githubusercontent.com/26136312/57781784-d62a2800-7765-11e9-95d7-467ce84ee740.png
    image_path: https://user-images.githubusercontent.com/26136312/57781784-d62a2800-7765-11e9-95d7-467ce84ee740.png
    alt: "예제 그림"
    title: "제너레이터 및 이터레이터"
---

<i class="fas fa-exclamation-circle"></i> 아래 문제는 **매일 프로그래밍**에서 가져온 문제입니다. **문제의 저작권은 매일프로그래밍에게 있습니다.** 해당 사이트에서 문제를 접할 수 있습니다. <a href="https://mailprogramming.com/" target="_blank">https://mailprogramming.com/</a>
{: .notice--danger}

## 문제

정수 배열(`int array`)이 주어지면 `0`이 아닌 정수 순서를 유지하며 모든 `0`을 배열 오른쪽 끝으로 옮기시오. 단, 시간복잡도는 `O(n)`, 공간복잡도는 `O(1)`여야 합니다.  

## 예제

```python
Input: [0, 5, 0, 3, -1]
Output: [5, 3, -1, 0, 0]
```

```python
Input: [3, 0, 3]
Output: [3, 3, 0]
```

## 문제 풀이

공간복잡도가 `O(1)`이다보니 제너레이터를 이용하여 풀면, 좋겠다는 생각을 했습니다.  

그리고 문제를 접근할 때, 한번 순회하면서 `0`은 무시하고 리스트를 만든 다음, 마지막에 맨 오른쪽에 `0`을 추가하는 방식으로 해서 편하게 풀 수 있었습니다.  

**참고** 해답을 보고 풀지 않았기 때문에, 해답과 문제 풀이가 다릅니다. 참고바랍니다.
{: .notice--warning}

### Python으로 풀기

파이썬에서는 제너레이터를 사용할 때, `yield` 키워드를 사용합니다.  

```python
# -*- coding: utf-8 -*-

def _generator(arr):
  """메모리 절약을 위해 제너레이터 이용"""
  for e in arr:
    if e != 0:
      yield e


def solution(arr):
  rst = list()
  for e in _generator(arr):
    rst.append(e)  # 이터레이터에서 0이 아닌 값을 가져옴

  rst.extend([0 for _ in range(len(rst), len(arr))])  # 나머지는 0으로 채움
  return rst


# Main
if __name__ == "__main__":
  test_case = [
    [0, 5, 0, 3, -1],
    [3, 0, 3]
  ]

  for test in test_case:
    print("Input:", test)
    print("Output:", solution(test))
```

```python
# 결과
Input: [0, 5, 0, 3, -1] 
Output: [5, 3, -1, 0, 0]
Input: [3, 0, 3]
Output: [3, 3, 0]
```

### 추가 설명

정리하면... 제너레이터는 **메모리를 효율적으로 사용**할 수 있다는 장점이 있죠. 그래서, 공간복잡도를 줄이기 위해서 제너레이터를 사용했습니다.  

그리고, 실제로 제너레이터를 이용했을 때, 사용하는 메모리가 줄어드는지 인터프리터에서 확인해보았습니다.  

```python
>>> import sys
>>> _list = [ i for i in range(1000) ]
>>> sys.getsizeof(_list)
4516
>>> sys.getsizeof(g_list(_list))  # generator
64
```

왜 이런 결과가 나오냐면, 제너레이터는 메모리를 한번에 적재해서 사용하는 것이 아니라 차례대로 하나씩 접근하여 메모리 공간을 사용하기 때문입니다.  

{% include gallery id="exam-gallery" caption="일반 리스트" %}

{% include gallery id="exam-gallery2" caption="제너레이터 및 이터레이터" %}


<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}