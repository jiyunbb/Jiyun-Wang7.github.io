---
author: Jiyun Wang
layout: post
title: mysql 설정 파일, my.cnf 확인하기
tags: [db, mysql]
---

개발DB가 죽어버렸다.  config 파일을 확인하고 싶다.


아래 명령어를 터미널에 쳤다.
```
$cd /etc/mysql/my.cnf
```

---

[mysqld]
```bash
#
# * Basic Settings
#
user            = mysql
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
port            = 3306
basedir         = /usr
datadir         = /var/lib/mysql
tmpdir          = /var/tmp
lc_messages_dir = /usr/share/mysql
lc_messages     = en_US
skip-external-locking
#
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
#
# * Fine Tuning
#
max_connections         = 1000
connect_timeout         = 5
wait_timeout            = 600
max_allowed_packet      = 16M
thread_cache_size       = 128
sort_buffer_size        = 1M
bulk_insert_buffer_size = 16M
tmp_table_size          = 128M
max_heap_table_size     = 128M


#
# * Logging and Replication
#
# Both location gets rotated by the cronjob.
# Be aware that this log type is a performance killer.
# As of 5.1 you can enable the log at runtime!
general_log_file        = /var/log/mysql/mysql.log    --> 돌아가는 쿼리들에 대한 로그를 쌓고 싶을 때! 로그 파일 지정
general_log             = 1    --> 0 = 하지 않겠다. / 1 = 사용하겠다. 이렇게 셋팅하고 mysql 재시작해주면 반영된다!
#
```


그 밖에 다양한 세부 세팅을 볼 수 있으니,
DB 에 문제가 생길 때 한 번 쯤은 들여다봐도 될 것 같다.
추가적인 정보는 알아가면서 해당 글에 덧붙여야 할 것 같다.
