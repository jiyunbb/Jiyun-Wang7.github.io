---
author: Jiyun Wang
layout: post
title: Array - Introduction
tags: [algorithm, data-structure]
---

참고 : [https://leetcode.com/explore/learn/card/fun-with-arrays](https://leetcode.com/explore/learn/card/fun-with-arrays)

릿코드 문제를 풀어보면서 해결해본 것들은 업데이트를 계속 진행하고 있습니다. (하단의 관련 포스트 참고)

## 배열
- 데이터를 나열하고, 각 데이터를 인덱스에 대응하도록 구성한 데이터 구조
- 파이썬에서는 리스트 타입이 배열 기능을 제공하고 있음


### 배열이 왜 필요할까?
---
- 같은 종류의 데이터를 효율적으로 관리하기 위해 사용
- 같은 종류의 데이터를 순차적으로 저장


### 장점
---
- 빠른 접근 가능


### 단점
---
- 추가/삭제가 쉽지 않음 -> 미리 지정된 사이즈보다 데이터를 더 추가해야하는 경우, 내부적으로 새로 더 큰 사이즈를 생성해서, 기존 아이템들을 옮겨가는 방식이다. (만약 용량이 6인 배열이 있는데, 7번째 아이템을 넣으려고 할 때, 위에 작성한 메커니즘으로 동작한다.)
- 미리 최대 길이(=용량)를 지정해야 함


### Array Capacity VS Length (용량과 길이의 차이는?)
---

There are two different answers you might have given.

	- The number of DVDs the box could hold, if it was full, or
	- The number of DVDs currently in the box.

Both answers are correct, and both have very different meanings! It's important to understand the difference between them, and use them correctly. We call the first one the capacity of the Array, and the second one the length of the Array.

**즉, 배열의 총 공간이 용량(capacity) 현재 배열이 사용된 양이 길이, 갯수(length)**

### 관련 포스트
---
- [Array - Introduction](https://jiyun-wang7.github.io/2021-03-31/array-introduction)
- [Array - Inserting items into an array](https://jiyun-wang7.github.io/2021-04-01/array-inserting-items-into-an-array)
- [Array - Deleting items from array](https://jiyun-wang7.github.io/2021-04-02/deleting-items-from-array)
- [Array - Searching for items in an array](https://jiyun-wang7.github.io/2021-04-04/searching-for-items-in-an-array)
