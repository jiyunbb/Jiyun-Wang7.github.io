---
author: Jiyun Wang
layout: post
title: Array - Inserting items into an array
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/525/inserting-items-into-an-array/3243/](https://leetcode.com/explore/learn/card/fun-with-arrays/525/inserting-items-into-an-array/3243/)
<br><br>

## 문제풀기
---

### 1. Max Consecutive Ones (연속된 1의 최대 갯수)
![Max Consecutive Ones)](/assets/array/1.png)

```python

# 1차 나의 작업물
def findMaxConsecutiveOnes(nums: List[int]) -> int:
    """
    Runtime: 372 ms
    Memory Usage: 14.6 MB
    """
    max_num = 0
    count = 0
    for num in nums:

        if num == 1:
            count += 1
        else:
            count = 0

        max_num = max(max_num, count)

    return max_num

# 2차 개선된 작업물(1차에 비해 속도와 메모리 감소 - max 함수를 쓰지 않으므로.)
def findMaxConsecutiveOnes(nums: List[int]) -> int:
    """
    Runtime: 332 ms
    Memory Usage: 14.5 MB
    """
    max_num = 0
    count = 0
    for num in nums:

        if num == 1:
            count += 1

        else:
            if count > max_num:
                max_num = count
            count = 0

        if count > max_num:
            max_num = count

    return max_num

# 3차 개선된 작업물(2차에서 중복 제거)
def findMaxConsecutiveOnes(nums: List[int]) -> int:
    """
    Runtime: 340 ms
    Memory Usage: 14.4 MB
    """
    max_num = 0
    count = 0
    for num in nums:

        if num == 1:
            count += 1

        else:
            count = 0

        if count > max_num:
            max_num = count

    return max_num

```

### 2. Find Numbers with Even Number of Digits (짝수 자리수를 가진 숫자 세기)
![Find Numbers with Even Number of Digits)](/assets/array/2.png)

```python

def findNumbers(nums: List[int]) -> int:
    """
    Runtime: 48 ms
    Memory Usage: 14.3 MB
    """
    count = 0
    for num in nums:
        number_of_digits = len(str(num)) // 참고 : str도 내부적으로는 배열로 취급할 수 있다.

        if number_of_digits % 2 == 0:
            count += 1

    return count

```

### 3. Squares of a Sorted Array (각 아이템 제곱 후 정렬)
![Find Numbers with Even Number of Digits)](/assets/array/3.png)

```python

def sortedSquares(nums: List[int]) -> List[int]:
    """
    Runtime: 228 ms
    Memory Usage: 16.2 MB
    """
    return sorted([num**2 for num in nums]) //제곱을 할 때, math.pow(값, 지수) 이렇게 쓸 수도 있음.

```
<br><br>

## 관련 포스트
---

- [Array - Introduction](https://jiyun-wang7.github.io/2021-03-31/array-introduction)
- [Array - Deleting items from array](https://jiyun-wang7.github.io/2021-04-02/deleting-items-from-array)
- [Array - Searching for items in an array](https://jiyun-wang7.github.io/2021-04-04/searching-for-items-in-an-array)
