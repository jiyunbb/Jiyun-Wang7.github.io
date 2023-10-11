---
author: Jiyun Wang
layout: post
title: binary search (문제풀이 정리 필요)
tags: [algorithm, data-structure]
---

## 이진 탐색

참고 : [https://leetcode.com/explore/learn/card/binary-search](https://leetcode.com/explore/learn/card/binary-search)

### 개념
- 이진 탐색이란 데이터가 정렬돼 있는 배열에서 특정한 값을 찾아내는 알고리즘이다. 배열의 중간에 있는 임의의 값을 선택하여 찾고자 하는 값 X와 비교한다. X가 중간 값보다 작으면 중간 값을 기준으로 좌측의 데이터들을 대상으로, X가 중간값보다 크면 배열의 우측을 대상으로 다시 탐색한다. 동일한 방법으로 다시 중간의 값을 임의로 선택하고 비교한다. 해당 값을 찾을 때까지 이 과정을 반복한다.

### 종료 조건 
- 원하는 값을 찾으면 종료.
- 탐색하고자 하는 배열이 더이상 존재하지 않으면 찾고자 하는 값이 배열에 존재하지 않는다는 것으로 판단할 수 있고 탐색을 종료.


### Distinguishing Syntax:

```python
Initial Condition: left = 0, right = length-1
Termination: left > right
Searching Left: right = mid-1
Searching Right: left = mid+1
```

### 구현

```python

def search(nums: [int], target: int) -> int:
    """
    재귀함수 사용하지 않음.
    """
    start_index = 0
    end_index = len(nums) - 1

    while (start_index <= end_index):
        mid_index = (start_index + end_index) // 2
        compare_index = nums[mid_index]

        if compare_index == target:
            return mid_index

        elif compare_index > target:
            end_index = mid_index - 1

        elif compare_index < target:
            start_index = mid_index + 1

    return -1


def search_recursive(nums: [int], target: int, start_index: int, end_index: int) -> int:
    """
    재귀함수 사용
    """
    if start_index > end_index:
        return -1

    mid_index = (start_index + end_index) // 2

    if nums[mid_index] == target:
        return mid_index

    elif nums[mid_index] > target:
        end_index = mid_index - 1
        return search_recursive(nums, target, start_index, end_index)

    elif nums[mid_index] < target:
        start_index = mid_index + 1
        return search_recursive(nums, target, start_index, end_index)

```


TODO 문제 푸는 거 추가하기!