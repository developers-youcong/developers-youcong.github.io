---
title: MySQL之集群配置
date: 2021-02-21 20:55:40
tags: "MySQL"
---

本次针对的MySQL版本为5.7，首先分别在A服务器和B服务器上安装MySQL，可以通过yum安装也可以通过wget下载直接编译安装。安装方式可以多种多样，但必须要确保安装成功。
<!--more-->
## 1.修改A服务器的my.cnf文件

```
vim /etc/my.cnf
```
并添加如下内容:
```
server-id=1
auto_increment_offset=1
auto_increment_increment=2
gtid_mode=on
enforce_gtid_consistency=on
log-bin=mysql-bin
```

## 2.修改B服务器的my.cnf文件
```
vim /etc/my.cnf
```
并添加如下内容:
```
server-id=2
auto_increment_offset=1
auto_increment_increment=2
gtid_mode=on
enforce_gtid_consistency=on
log-bin=mysql-bin

```

## 3.在A服务器上的MySQL创建B服务器访问的复制用户
```
create user B@'IP' identified by '密码';
grant replication slave on *.* to B@'服务器IP';

```

## 4.在B服务器上的MySQL创建A服务器访问的复制用户
```
create user A@'IP' identified by '密码';
grant replication slave on *.* to A@'密码';

```

## 5.在B服务器上的MySQL执行主从配置，进行A主B从
```
change master to master_host='IP', master_user='B', master_password='?T-p&clsr38i', master_port=3306, master_auto_position=1;

start slave;

show slave status;
```

## 6.在A服务器上的MySQL执行主从配置，进行B主A从
```
change master to master_host='IP', master_user='A', master_password='?T-p&clsr38i', master_port=3306, master_auto_position=1;

start slave;

show slave status;
```

然后测试，在A服务器上的MySQL新建数据库和对应的数据表，B服务器上的MySQL会同步过来，确保数据库和数据表一致。

## 7.Nginx配置
Nginx配置MySQL集群访问URL，确保微服务应用连接相同的URL。
Nginx中的MySQL配置，内容如下:
```
stream {
    upstream mysql_proxy{
       hash $remote_addr consistent;
       server A服务器IP:3306 weight=1 max_fails=3 fail_timeout=10s;
	   server B服务器IP:3306 weight=1 max_fails=3 fail_timeout=10s;
    }
    server {
       listen 3306; # 数据库服务器监听端口
       proxy_connect_timeout 10s;
       proxy_timeout 300s; 
       proxy_pass mysql_proxy;
    }
}
```
**特别注意:**
生产环境不建议设置MySQL端口为3306或3389。

关于Nginx配置MySQL可以参考我写的这篇文章:
[nginx之MySQL代理](https://youcongtech.com/2021/01/22/nginx%E4%B9%8BMySQL%E4%BB%A3%E7%90%86/)