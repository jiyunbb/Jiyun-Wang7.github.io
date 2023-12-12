---
author: Jiyun Wang
layout: post
title: Array - Replace Elements with Greatest Element on Right Side
tags: [algorithm, data-structure]
---

## 참고
---
[https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3259/](https://leetcode.com/explore/learn/card/fun-with-arrays/511/in-place-operations/3259/)
<br><br>

## 문제풀기
---

### 1. Remove Element (특정 값들을 배열에서 제거)
![Remove Element)](/assets/array/10.png)

```python

class Solution:
    def replaceElements(self, arr: List[int]) -> List[int]:
        results = [0] * len(arr)
        results[-1] = -1
        for i in range(len(arr) - 2, -1, -1):
            # 오른쪽에서 부터 순회
            # https://dev.to/kira/leetcode-1299-replace-elements-with-greatest-element-on-right-side-47me
            results[i] = max([results[i + 1], arr[i + 1]])

        return results

```
