---
title: >-
  ERROR 1820 (HY000): You must reset your password using ALTER USER statement
  before executing this statement.
date: 2019-04-23 23:10:04
tags: "MySQL"
---
错误信息如下:
ERROR 1820 (HY000): You must reset your password using ALTER USER statement
before executing this statement.

翻译过来的意思是:
错误1820 (HY000):您必须使用ALTER USER语句重置密码
在执行此语句之前。
<!--more-->
所以解决办法就是使用重置密码命令:
```
alter user 'root'@'localhost' identified by 'youpassword';  

```
参考资料如下:
[mysql 报错ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executin](https://www.cnblogs.com/lmx123/p/9321792.html)
[MySQL 8.0.15安装教程(windows 64位)](https://blog.csdn.net/qq_37350706/article/details/81707862)
