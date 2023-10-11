---
author: Jiyun Wang
layout: post
title: Array - Searching for items in an array
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/527/searching-for-items-in-an-array/3296/](https://leetcode.com/explore/learn/card/fun-with-arrays/527/searching-for-items-in-an-array/3296/)
<br><br>

## 선형 검색 (linear Search)
---
- 인덱스를 알 수없는 경우 (대부분의 경우) Array의 모든 요소를 ​​확인해야 할 수 있다. 찾고있는 요소를 찾거나 배열의 끝에 도달 할 때까지 계속 요소를 확인한다. 즉, 모든 요소를 ​​하나씩 확인하여 요소를 찾는이 기술을 선형 검색 알고리즘이라고 한다. 
- 최악의 경우 선형 검색은 전체 Array를 확인하게 된다. 따라서 선형 검색의 시간 복잡도는 O(N) 이며, N은 배열의 길이(length)를 의미한다.

```python
from typing import List


def linear_search(arr: List[int], target: int) -> bool:
    if not arr:
        return False

    for item in arr:
        if item == target:
            return True

    return False


if __name__ == '__main__':
    arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    target = 10
    is_exists = linear_search(arr=arr, target=target)
    print(is_exists)

```

## 이진 검색 (Binary Search)
---
- 배열의 요소가 정렬 된 순서이면 이진 검색을 사용할 수 있다. 이진 검색은 배열의 중간 요소를 반복적으로보고 찾고있는 요소가 왼쪽인지 오른쪽인지 결정한다. 
- 이 작업을 수행 할 때마다 검색해야하는 요소 수를 절반으로 줄일 수 있으므로 이진 검색이 선형 검색보다 훨씬 빠르다는 장점이 있다. 
- 이진 검색의 단점은 데이터가 정렬 된 경우에만 작동한다. 단일 검색 만 수행해야하는 경우 선형 검색보다 정렬하는 데 더 오래 걸리므로 선형 검색 만 수행하는 것이 더 빠릅니다. 많은 검색을 수행하려는 경우 반복 검색에 이진 검색을 사용할 수 있도록 데이터를 먼저 정렬하는 것이 좋다.

### 이전에 정리해둔 이진검색 (아직 문제풀이가 업데이트 되어있지 않지만, 구현은 해둠.)
- [https://jiyun-wang7.github.io/2021-03-29/binary-search](https://jiyun-wang7.github.io/2021-03-29/binary-search)

<br><br>


## 문제풀기
---
### 1. Check If N and Its Double Exist (특정 값의 곱하기 2한 값이 배열안에 있는지 확인)
![Check If N and Its Double Exist)](/assets/array/8.png)

```python

def checkIfExist(arr: List[int]) -> bool:
    """
    반복문 돌면서 전체 비교 (1차 작업)
    Runtime: 68 ms
    Memory Usage: 14.4 MB
    """
    if not arr:
        return False

    for i in range(0, len(arr)):
        item_divided_half = arr[i] / 2

        for j in range(0, len(arr)):
            if j == i:
                continue

            if item_divided_half == arr[j]:
                return True

    return False


def checkIfExist(arr: List[int]) -> bool:
    """
    반복문 돌면서 값을 set에 저장해두고, set을 기준으로 비교. (2차 작업)
    Runtime: 48 ms
    Memory Usage: 14.4 MB
    """
    if not arr:
        return False

    doubles = set()

    for i in range(0, len(arr)):
        if arr[i] * 2 in doubles or arr[i] / 2 in doubles:
            return True

        doubles.add(arr[i])

    return False

```


### 2. Valid Mountain Array (각 아이템의 흐름이 산 모양인지 확인)
![Valid Mountain Array)](/assets/array/9.png)

```python

def validMountainArray(arr: List[int]) -> bool:
    """
    1차 작업물
    Runtime: 204 ms
    Memory Usage: 15.6 MB
    """
    if not arr or len(arr) < 3:
        return False

    # 올라간 적이 있다.
    is_up = False

    # 내려간 적이 있다.
    is_down = False

    for i in range(1, len(arr)):
        if arr[i - 1] == arr[i]:
            return False

        elif arr[i - 1] < arr[i]:
            if is_down:
                # 내려갔었다가 다시 올라가는 경우
                return False
            is_up = True

        elif arr[i - 1] > arr[i]:
            is_down = True

    if is_up and is_down:
        return True

    return False


def validMountainArray(arr: List[int]) -> bool:
    """
    2차 작업물(개선)
    Runtime: 204 ms
    Memory Usage: 15.4 MB
    """
    if not arr or len(arr) < 3 or (arr[0] >= arr[1]):
        return False

    # 올라간 적이 있다.
    is_up = True

    for i in range(1, len(arr)):
        if arr[i - 1] == arr[i]:
            return False

        if is_up:
            # 올라가는 상황에서 내려가야하는 경우
            if arr[i - 1] > arr[i]:
                is_up = False

        if not is_up:
            # 올라가는 상황이 끝났는데, 올라간다.
            if arr[i - 1] < arr[i]:
                return False

    return not is_up # 결국 내려가야 하기 때문에.

```


## 관련 포스트
---
- [Array - Introduction](https://jiyun-wang7.github.io/2021-03-31/array-introduction)
- [Array - Inserting items into an array](https://jiyun-wang7.github.io/2021-04-01/array-inserting-items-into-an-array)
- [Array - Deleting items from array](https://jiyun-wang7.github.io/2021-04-02/deleting-items-from-array)


