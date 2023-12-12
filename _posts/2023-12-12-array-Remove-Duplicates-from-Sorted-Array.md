---
author: Jiyun Wang
layout: post
title: Array - Remove Duplicates from Sorted Array
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3258/](https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3258/)
<br><br>

## 문제풀기
---

![Remove Duplicates from Sorted Array](/assets/array/11.png)
1. [In-Place 알고리즘](https://en.wikipedia.org/wiki/In-place_algorithm) 사용
2. Two pointers 알고리즘 사용
   - 리스트에 순차적으로 접근해야 할 때 두 개의 점의 위치를 기록하면서 처리하는 알고리즘
정렬되어있는 두 리스트의 합집합에도 사용됨. 
   - 병합정렬(merge sort)의 counquer 영역의 기초가 되기도 함.

```python

class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        i = 0
        for j in range(1, len(nums)):
            if nums[i] != nums[j]:
                i += 1
                nums[i] = nums[j]

        return i+1

```
