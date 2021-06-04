---
title: centos7.8之时间不对问题
date: 2020-09-14 19:55:45
tags: "Linux"
---

按照如下命令操作，即可解决问题:
```
yum install -y ntpdate

ntpdate us.pool.ntp.org

date

```
完成这三条命令后，时间就回归正常了。
[修改centos系统时间不对的问题](https://blog.csdn.net/king_wang10086/article/details/76178711)