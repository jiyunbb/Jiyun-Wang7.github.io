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

![Replace Elements with Greatest Element on Right Side](/assets/array/10.png)
1. [In-Place 알고리즘](https://en.wikipedia.org/wiki/In-place_algorithm) 사용
2. Two pointers 알고리즘 사용
   - 리스트에 순차적으로 접근해야 할 때 두 개의 점의 위치를 기록하면서 처리하는 알고리즘
정렬되어있는 두 리스트의 합집합에도 사용됨. 
   - 병합정렬(merge sort)의 counquer 영역의 기초가 되기도 함.

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
