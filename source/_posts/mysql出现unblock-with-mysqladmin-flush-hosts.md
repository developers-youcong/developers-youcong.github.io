---
title: mysql出现unblock with 'mysqladmin flush-hosts'
date: 2020-09-30 17:26:52
tags: "MySQL"
---
错误信息:
```
unblock with 'mysqladmin flush-hosts'

```
这个错误导致我无法远程连接MySQL(使用navicat或sqlyog等mysql客户端工具)
<!--more-->

解决办法:
修改max_connect_errors的值
```
(1)进入Mysql数据库查看max_connect_errors：
> show variables like '%max_connect_errors%';
(2)修改max_connect_errors的值：
> set global max_connect_errors = 100;
(3)查看是否修改成功
> show variables like '%max_connect_errors%';

```
参考资料:
[mysql出现unblock with 'mysqladmin flush-hosts'](https://www.cnblogs.com/abclife/p/9469622.html)
