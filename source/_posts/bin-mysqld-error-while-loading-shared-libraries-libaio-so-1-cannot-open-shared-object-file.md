---
title: >-
  /bin/mysqld: error while loading shared libraries: libaio.so.1: cannot open
  shared object file:
date: 2021-02-16 16:25:11
tags: "Linux"
---

错误详细信息:
```
./mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory

```

错误原因分析:
是因为缺少初始化所必须的库。


解决办法:
安装对应的库，即可，执行如下命令:
```
yum install -y libaio

```