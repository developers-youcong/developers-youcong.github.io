---
title: '请在mysql配置文件修sql-mode或sql_mode为NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'
date: 2019-05-06 16:26:05
tags: "MySQL"
---

错误信息:请在mysql配置文件修sql-mode或sql_mode为NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

解决办法(最有效，以MySQL5.7为例):
修改配置文件
```
vim /etc/mysql/mysql.conf.d/mysqld.cnf

```
并在该配置文件添加如下内容:
```
sql-mode = NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

```

最后，重启一下MySQL服务器即可解决该问题
<!--more-->