m---
author: Jiyun Wang
layout: post
title: mysql binlog 확인하기
tags: [db, mysql]
---

### binlog를 확인하는 목적 : 내가 원하는 시간에 DB에서 어떠한 쿼리가 돌아갔는지 확인 
- 히스토리 관리
- 이전에 발생된 문제상황 예측 가능


1)  .ssh로 DB에 접속한 뒤에, 해당 디렉토리에 이동한다.
```
$ cd /data/var/log/mysql binlog
```

2) 내가 원하는 시간대를 보기 위해 해당 커맨드를 날려서 찾을 것이다.
```
$ll
```


예시 ) 
```
-rw-rw----  1 mysql adm    104857699 Nov  1 13:04 mariadb-bin.008113
-rw-rw----  1 mysql adm    104857978 Nov  1 15:08 mariadb-bin.008114
-rw-rw----  1 mysql adm    104857865 Nov  1 17:10 mariadb-bin.008115
-rw-rw----  1 mysql adm     73672766 Nov  1 18:26 mariadb-bin.008116
```

3) 원하는 시점의 bin log를 찾는다. (Vim, grep으로 이용하면 된다.)
```
$ vi mariadb-bin.008113
```


정리가 잘 되어있진 않지만, 어느 정도 쿼리를 파악하고 문제상황들을 예측할 수 있다.
더 나아가 사용자가 어떠한 행동을 했는지도 파악이 가능하다.