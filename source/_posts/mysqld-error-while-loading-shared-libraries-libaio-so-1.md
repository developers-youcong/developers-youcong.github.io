---
title: 'mysqld: error while loading shared libraries: libaio.so.1'
date: 2021-06-20 22:29:41
tags: "MySQL"
---
CentOS7.x版本初始化MySQL,出现如下错误:
```
mysqld: error while loading shared libraries: libaio.so.1:cannot open shared object file: No such file or directory

```
<!--more-->
原因是因为:新的服务器没有安装所需依赖导致的。

解决办法，执行如下命令，安装所需依赖即可:
```
yum install -y libaio

```