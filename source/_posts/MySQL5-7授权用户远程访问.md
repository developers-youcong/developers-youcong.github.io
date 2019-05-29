---
title: MySQL5.7授权用户远程访问
date: 2019-05-15 21:55:04
tags: "MySQL"
---
做个记录，每次弄环境的时候，特别是弄mysql环境，时不时都要用到下面的命令

命令如下:
```
    grant all privileges on *.* to 'root'@'%' identified by 'oa123456' with grant option;
    flush privileges;
    quit;

```
<!--more-->


**注意:**
上面的命令原型如下:
```
 grant all privileges on *.* to 'username'@'%' identified by 'password' with grant option;

```
命令中的“%”相当于授权任意主机。

另外还有就是通常授权用户远程连接，还需要修改配置文件，以Ubuntu16.04为例，需要修改mysqld.cnf配置文件，将里面的bind=127.0.0.1注释掉即可
然后重启一下mysql服务，这时你就可以通过sqlyong或navicat连接mysql服务。