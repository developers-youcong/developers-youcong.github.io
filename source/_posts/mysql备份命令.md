---
title: mysql备份命令
date: 2019-03-21 13:32:20
tags: "MySQL"
---

mysql备份命令如下:
<!--more-->
```
备份多个数据库可以使用如下命令:
mysqldump -uroot -p123456 --databases test1 test2 test3 > /home/test/dump.sql;

恢复备份:
source dump.sql 在mysql命令行中输入该命令即可恢复

备份整个数据库:
 mysqldump -uroot -123456 -A > all.sql

备份整个数据库结构:
 mysqldump -uroot -p123456 -P3306 -A -d > all_002.sql
 
备份单个数据库结构及其数据
mysqldump -uroot -p123456 -P3306 test > all_003.sql


备份单个数据库结构及其数据
mysqldump -uroot -p123456 -P3306 test -d > all_004.sql
备份单个数据库数据
mysqldump -uroot -p123456 -P3306 test -t > all_005.sql

```

通常情况下，备份数据库的结构和数据，在实际生产环境中用的比较多，对于大数据时代而言，数据是至关重要的，通过数据分析便可发现用户某些行为，从而开辟市场
