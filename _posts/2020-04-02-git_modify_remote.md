---
author: Jiyun Wang
layout: post
title: git remote 변경하기 (for fork)
tags: [git]
---

- 기존 디렉토리를 fork된 리포지토리를 바라보게 하기 위해 사용했다.

### **기존 리포지토리 깔끔하게 pull / push**

```
git pull
git add .
git commit -m "clean push"
git push

```

### **기존 리포지토리 remote 제거**

```
git remote remove origin

```

### **새 리포지토리 remote 추가**

```
git remote add origin https://github.com/계정/리포지토리
```