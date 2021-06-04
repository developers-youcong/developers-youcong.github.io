---
title: 'ERROR 1006 (HY000): Can''t create database'
date: 2021-01-11 21:47:31
tags: "Linux"
---

错误信息:
```
ERROR 1006 (HY000): Can''t create database
```
<!--more-->
错误原因(针对编译安装MySQL而非直接安装MySQL):
与授权有关，我改了mysql所在目录下的权限，导致我创建数据库失败。

解决办法:
重新授权。如chown -R mysql /usr/software/mysql。

参考链接:
[MySQL: 1006 - Can't create database '***' (errno: 13) 错误 解决方法](https://blog.csdn.net/kexiaoling/article/details/50259569)


