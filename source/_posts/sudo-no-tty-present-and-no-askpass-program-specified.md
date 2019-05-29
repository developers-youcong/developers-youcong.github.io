---
title: 'sudo: no tty present and no askpass program specified'
date: 2019-04-27 22:52:15
tags: "jenkins"
---

错误信息:
```
sudo: no tty present and no askpass program specified

```

错误原因:
是由于帐号并没有开启免密码导致的 

解决办法:

编辑sudoers文件

vim /etc/sudoers

添加免密码:
```
用户名 ALL = NOPASSWD: ALL

```

如:jenkins ALL = NOPASSWD: ALL

参考链接:
[sudo: no tty present and no askpass program specified 解决方法](https://blog.csdn.net/a_little_a_day/article/details/78282983)
