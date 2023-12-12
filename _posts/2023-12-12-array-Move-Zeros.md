---
author: Jiyun Wang
layout: post
title: Array - Replace Elements with Greatest Element on Right Side
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3157/](https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3157/)
<br><br>

## 문제풀기
---

### 1. Remove Element (특정 값들을 배열에서 제거)
![Remove Element)](/assets/array/12.png)

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
