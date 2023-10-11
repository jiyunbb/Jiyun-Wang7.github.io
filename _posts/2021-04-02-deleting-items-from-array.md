---
author: Jiyun Wang
layout: post
title: Array - Deleting items from array
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/526/deleting-items-from-an-array/3246/](https://leetcode.com/explore/learn/card/fun-with-arrays/526/deleting-items-from-an-array/3246/)
<br><br>

## 문제풀기
---

### 1. Remove Element (특정 값들을 배열에서 제거)
![Remove Element)](/assets/array/6.png)

```python

def removeElement(self, nums: List[int], val: int) -> int:
    """
    Runtime: 32 ms
    Memory Usage: 14 MB
    큐처럼 생각하기
    비교는 가장 0번 인덱스 값
    일치하지 않을 경우 뒤로 보냄.
    """
    for index in range(len(nums)):
        if nums[0] != val:
            nums.append(nums[0])

        nums.pop(0)

    return len(nums)

def removeElement(self, nums: List[int], val: int) -> int:
    """
    Runtime: 28 ms
    Memory Usage: 14.2 MB
    계속 반복문을 돌면서, val이 nums안에서 없어져서 flag가 false 일 때 까지!
    """
    flag = True
    while flag:
        if val in nums:
            flag = True
            nums.remove(val)
        else:
            flag = False
    return len(nums)

```


### 2. Remove Duplicates from Sorted Array (특정 값들을 정렬된 배열에서 제거)
![Remove Duplicates from Sorted Array)](/assets/array/7.png)

```python

    def removeDuplicates(self, nums: List[int]) -> int:
        """
        Runtime: 3804 ms
        Memory Usage: 16.1 MB

        count가 1이 될 때 까지 while로 돈다. 런타임 시간이...
        """
        for num in nums:
            while nums.count(num) > 1:
                nums.remove(num)

        return len(nums)

    def removeDuplicates(self, nums: List[int]) -> int:
        """
        Runtime: 72 ms
        Memory Usage: 16 MB

        앞 수와 뒷 수를 비교해서 다르면 그때 중복이 끝났다고 생각하고 뒷 수를 앞 수로 엎어치기.
        예) 0, 0, 1 -> 0,0은 같으니 pass / 0, 1은 다르니 중복이 끝났다고 생각해서 엎어치기 -> 0,1,1
        :param nums:
        :return:
        """
        index = 1
        for i in range(0, len(nums) - 1):
            if nums[i] != nums[i + 1]:
                nums[index] = nums[i + 1]
                index += 1

        return index

```


## 관련 포스트
---
- [Array - Introduction](https://jiyun-wang7.github.io/2021-03-31/array-introduction)
- [Array - Inserting items into an array](https://jiyun-wang7.github.io/2021-04-01/array-inserting-items-into-an-array)
- [Array - Searching for items in an array](https://jiyun-wang7.github.io/2021-04-04/searching-for-items-in-an-array)


