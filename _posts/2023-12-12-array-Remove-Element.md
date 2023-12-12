---
author: Jiyun Wang
layout: post
title: Array - Remove Element
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3575/](https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3575/)
<br><br>

## 문제풀기
---

![Sort Array By Parity](/assets/array/14.png)

```python

class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        while nums.count(val):
            nums.remove(val)
        return len(nums)

```
