---
title: Ubuntu16.04忘记MySQL5.7的root用户密码之解决方案
date: 2019-05-07 21:08:13
tags: "MySQL"
---

其实也就四步，如下:
<!--more-->
## 修改配置文件
```
sudo vimi /etc/mysql/mysql.conf.d/mysqld.cnf

```

并在 在[mysqld]下方的skip-external-locking下面添加一行：

```
skip-grant-tables

```

然后再重启MySQL
```
/etc/init.d/mysql restart

```


## 进入MySQL修改密码

```
mysql -uroot -p

```
一路回车，免密登录

设置密码
```
UPDATE mysql.user SET authentication_string=password('kdfaslf') WHERE User='root' AND Host ='localhost';

```


刷新权限
```
flush privileges;

```

退出
```
quit

```

## 修改配置文件(sudo vimi /etc/mysql/mysql.conf.d/mysqld.cnf)
并将在[mysqld]下方的skip-external-locking下面添加一行的skip-grant-tables去除

## 重启MySQL让配置生效，回归正常
```
/etc/init.d/mysql restart

```
参考资料如下:
[Ubuntu 16.04下忘记MySQL密码解决方法](https://blog.csdn.net/mr_hui_/article/details/83011202)