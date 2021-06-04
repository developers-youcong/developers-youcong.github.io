---
title: 'error while loading shared libraries: libluajit-5.1.so.2'
date: 2020-12-02 21:36:22
tags: "Linux"
---

错误详细信息:
```
./sbin/nginx: error while loading shared libraries: libluajit-5.1.so.2: 
cannot open shared object file: No such file or directory


```
<!--more-->
错误背景:
搭建nginx+lua环境，启动nginx遇到这样的错误。

错误原因:
nginx中有lua相关库需要有lua环境才能运行起来。

解决错误，核心代码(安装lua环境即可):
```
yum -y install lua*

```

参考解决问题链接:
[/usr/local/nginx/sbin/nginx: error while loading shared libraries: libluajit-5.1.so.2: cannot open](https://blog.csdn.net/weixin_45093060/article/details/104037482)