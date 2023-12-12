---
author: Jiyun Wang
layout: post
title: Array - Sort Array By Parity
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3260/](https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3260/)
<br><br>

## 문제풀기
---

![Sort Array By Parity](/assets/array/13.png)

```python

class Solution:
    def sortArrayByParity(self, nums: List[int]) -> List[int]:
        for i in range(len(nums)-1, -1, -1):
            if nums[i] % 2 != 0:
                nums.append(nums.pop(i))

            i += 1

        return nums

```
