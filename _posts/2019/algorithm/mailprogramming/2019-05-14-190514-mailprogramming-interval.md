---
title: "매일프로그래밍 알고리즘, 간격의 합집합 구하기"
excerpt: "본 글은 '매일프로그래밍'의 알고리즘 문제를 풀이하는 내용이 들어있습니다. 문제에 대한 저작권은 '매일프로그래밍'에게 있습니다. 문제는 다음과 같습니다. Given a list of intervals, merge intersecting intervals."
search: true
categories: 
  - Algorithm
  - Mailprogramming
tags: 
  - 알고리즘
  - Python
  - 파이썬
  - 매일프로그래밍
last_modified_at: 2019-05-14T22:19:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190514-mailprogramming-interval.png
exam-gallery:
  - url: https://user-images.githubusercontent.com/26136312/57702608-6484a800-7699-11e9-8948-9062ebf1be68.png
    image_path: https://user-images.githubusercontent.com/26136312/57702608-6484a800-7699-11e9-8948-9062ebf1be68.png
    alt: "예제 그림"
    title: "2 ~ 4 구간 색칠"
exam-gallery2:
  - url: https://user-images.githubusercontent.com/26136312/57702609-6484a800-7699-11e9-8aa2-14d772a59009.png
    image_path: https://user-images.githubusercontent.com/26136312/57702609-6484a800-7699-11e9-8aa2-14d772a59009.png
    alt: "예제 그림2"
    title: "1 ~ 5 구간 색칠"
exam-gallery3:
  - url: https://user-images.githubusercontent.com/26136312/57702612-651d3e80-7699-11e9-9132-b6f52162b1f8.png
    image_path: https://user-images.githubusercontent.com/26136312/57702612-651d3e80-7699-11e9-9132-b6f52162b1f8.png
    alt: "예제 그림3"
    title: "7 ~ 9 구간 색칠"
exam-gallery4:
  - url: https://user-images.githubusercontent.com/26136312/57702937-fee4eb80-7699-11e9-8e73-b0cd487a91f6.png
    image_path: https://user-images.githubusercontent.com/26136312/57702937-fee4eb80-7699-11e9-8e73-b0cd487a91f6.png
    alt: "예제 그림"
    title: "비트마스크로 변환"
---

<i class="fas fa-exclamation-circle"></i> 아래 문제는 **매일 프로그래밍**에서 가져온 문제입니다. **문제의 저작권은 매일프로그래밍에게 있습니다.** 해당 사이트에서 문제를 접할 수 있습니다. <a href="https://mailprogramming.com/" target="_blank">https://mailprogramming.com/</a>
{: .notice--danger}

## 문제

간격(`interval`)로 이루어진 배열이 주어지면, 겹치는 간격 원소들을 합친 새로운 배열을 만드시오. 간격은 시작과 끝으로 이루어져 있으며 시작은 끝보다 작거나 같습니다.  

Given a list of intervals, merge intersecting intervals.  

## 예제

```python
Input: { { 2, 4 }, { 1, 5 }, { 7, 9 } }
Output: { { 1, 5 }, { 7, 9 } }
```

```python
Input: { { 3, 6 }, { 1, 3 }, { 2, 4 } }
Output: { { 1, 6 } }
```

## 문제 풀이

해당 문제를 풀때는 그림을 그려서 생각해봤습니다. 각각의 구간을 색칠하였고 나중에는 색칠된 모든 구간의 합을 구했습니다.  

{% include gallery id="exam-gallery" caption="2 ~ 4 구간 색칠" %}

{% include gallery id="exam-gallery2" caption="1 ~ 5 구간 색칠" %}

{% include gallery id="exam-gallery3" caption="7 ~ 9 구간 색칠" %}

위와 같이 풀기 위해 비트마스크를 활용했습니다. 각각의 원소 중 가장 큰 수까지 **비트마스크**를 만들어서 각각의 상태를 `0` 또는 `1`로 저장해둡니다.  

- 0: 색칠이 안된 구간(점)
- 1: 색칠된 구간(점)

{% include gallery id="exam-gallery4" caption="비트마스크로 표현했을때" %}

위 방식으로 풀면 좋은점은 시간복잡도 `O(n)`으로 풀 수 있습니다. 하지만 **단점으로는 저장공간이 많아진다는 점**입니다.


### Python으로 풀기

우선 적당량의 리스트를 만들어주기 위해, 가장 큰 원소의 값을 구합니다. 이때 `max()` 함수를 이용했습니다.  

가장 큰 원소의 값만큼 리스트의 크기를 결정해줍니다.  

000`01`111`10`0 과 같이 `01`이 있는 부분은 시작점이고, `10`은 끝나는점 입니다. 이 점을 이용하여 분기(`if`)를 나눠서 결과를 도출합니다.  

```python
# -*- coding: utf-8 -*-

def solution(arr):
  # 최댓값을 구함
  max_num = float("-inf")  # 가장 작은 값
  for e in arr:
    max_num = max(max_num, max(e[0], e[1]))

  # 최대값만큼 리스트를 생성
  flag = [0 for _ in range(max_num+1)]
  for e in arr:
    # 비트를 1로 바꿔줌. (그래프를 그림)
    for idx in range(e[0], e[1]+1):
        flag[idx] = 1

  # 결과값 반환
  rst, tmp_range = list(), list()
  for idx in range(max_num):
    if flag[idx] == 0 and flag[idx+1] == 1:
        tmp_range.append(idx+1)  # 처음 시작점
    elif (flag[idx] == 1 and flag[idx+1] == 0):
        tmp_range.append(idx)  # 끝나는점
    elif (idx == max_num-1 and flag[idx] == 1 and flag[idx+1] == 1):
        tmp_range.append(idx+1)  # 맨 마지막점인데, 값이 1인경우

    # 구간이 만들어지면 결과값에 넣음
    if len(tmp_range) == 2:
        rst.append(tmp_range)
        tmp_range = list()
  return rst

# main
if __name__ == "__main__":
  test_case = [[[2, 4], [1, 5], [7, 9]], [[3, 6], [1, 3], [2, 4]]]
  for test in test_case:
    print("Input:", test)
    print("Output:", solution(test))
```

```python
# 결과
Input: [[2, 4], [1, 5], [7, 9]]
Output: [[1, 5], [7, 9]]
Input: [[3, 6], [1, 3], [2, 4]]
Output: [[1, 6]]
```

사실 위 방법은 mailprogramming에서 나온 해답과는 다른 풀이입니다. 참고하시길 바랍니다.  

<i class="far fa-laugh-wink"></i> 문제의 풀이 방법은 매우 다양합니다. 좀 더 효율적이고 좋은 방법이 있다면 **댓글**이나 **이메일**로 알려주세요. 같이 문제 풀어봅시다 :)
{: .notice--success}