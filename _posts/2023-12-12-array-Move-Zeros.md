---
author: Jiyun Wang
layout: post
title: Array - Move Zeros
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3157/](https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3157/)
<br><br>

## 문제풀기
---

![Move Zeros](/assets/array/12.png)
1. [In-Place 알고리즘](https://en.wikipedia.org/wiki/In-place_algorithm) 사용
2. Two pointers 알고리즘 사용
   - 리스트에 순차적으로 접근해야 할 때 두 개의 점의 위치를 기록하면서 처리하는 알고리즘
정렬되어있는 두 리스트의 합집합에도 사용됨. 
   - 병합정렬(merge sort)의 counquer 영역의 기초가 되기도 함.

```python

class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i = 0
        for j in range(len(nums)-1, -1, -1):
            # i는 앞에서부터, j는 뒤에서부터 순회
            if nums[j] == 0:
                temp = nums.pop(j)
                nums.append(temp)

            if nums[i] == 0:
                temp = nums.pop(i)
                nums.append(temp)

            i += 1

```
