---
author: Jiyun Wang
layout: post
title: Array - Searching for items in an array (작성중)
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3257/](https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3257/)
<br><br>

## In-Place Array
---
- In-place Array 작업은 공간 복잡성을 줄이는 데 도움이 된다.
- 아래 샘플 : 정수 배열이 주어지면 짝수 인덱스 위치에있는 모든 요소가 제곱되는 배열을 반환하기.
```
Input: array = [9, -2, -9, 11, 56, -12, -3]
Output: [81, -2, 81, 11, 3136, -12, 9]
Explanation: The numbers at even indexes (0, 2, 4, 6) have been squared, 
whereas the numbers at odd indexes (1, 3, 5) have been left the same.
```
- 방법 2가지
    + 새로운 배열을 생성하고, 짝수는 제곱해서 넣고, 홀수는 복사해서 넣기
        * 공간 복잡도 면에서 비효율 적임. O(length) 만큼 새로운 공간이 필요하다.
    + 기존 배열에서 짝수를 제곱해서 덮어쓰기. (추가공간이 필요없어서 효율)
        * 우리가 in-place라고 부르는 새로운 배열을 생성하지 않고 입력 배열에서 직접 작업하는 기술이다. in-place array 작업은 알고리즘의 시간 및 공간 복잡성을 최소화하는 데 중점을 둔다.
        * 다만 덮어써진 데이터(기존 데이터)는 더이상 접근이 불가하다.
- 최대한 문제를 풀 때 새로운 공간을 사용하지 않도록 노력해야 할 듯 하다.

코드 구현
```python

def evenSquared(arr: List[int]) -> List[int]:
    for i in range(len(arr)):
        if i % 2 == 0:
            arr[i] = arr[i] ** 2

    return arr

```


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


