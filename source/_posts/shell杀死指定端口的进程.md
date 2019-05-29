---
title: shell杀死指定端口的进程
date: 2019-04-13 15:23:07
tags: "Linux"
---

杀死端口代码如下:
```
lsof -i:2019

kill -9 PID

```
<!--more-->
上面的与下面的代码作用相同。

命令如下所示(这种方式更自动化):
```
kill -9 $(netstat -nlp | grep :2019 | awk '{print $7}' | awk -F"/" '{ print $1 }')

```
