---
title: centos7.8安装rabbitmq
date: 2020-09-18 20:11:02
tags: "Linux"
---

以安装3.7.28为例，步骤如下:
<!--more-->
## 一、安装erlang
```
curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash

yum install -y erlang


erl -version

```

## 二、安装rabbitmq
```
wget https://dl.bintray.com/rabbitmq/all/rabbitmq-server/3.7.28/rabbitmq-server-3.7.28-1.el7.noarch.rpm

yum install rabbitmq-server-3.7.28-1.el7.noarch.rpm

## 开启管理后台界面
rabbitmq-plugins enable rabbitmq_management

## 必须执行，否则会导致启动rabbitmq-server失败
chown rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie


```

## 三、rabbitmq常用命令
```
#前台启动服务
rabbitmq-server
 
#后台启动服务
rabbitmq-server -detached 
 
#停止服务
rabbitmqctl stop 
 
#查看状态
rabbitmqctl status 

```

## 四、rabbitmq添加用户
```
#添加账户，用户名test 密码123456
rabbitmqctl add_user test 123456
 
#授予用户角色，总共有四种角色，这里授予的是administrator
rabbitmqctl set_user_tags test administrator
 
#设置用户允许访问的vhost
rabbitmqctl set_permissions -p /  test '.*' '.*' '.*'


```

## 五、注意事项(常见问题)

错误信息1:
```
/usr/lib/rabbitmq/bin/rabbitmq-server:行51: /var/lib/rabbitmq/mnesia/rabbit@test.pid: 权限不够

```

解决办法:
```
chown -R rabbitmq:rabbitmq /var/lib/rabbitmq/mnesia/

```

错误信息2:
```
启动rabbitmq：ERROR: distribution port 25672 in use on localhost (by non-Erlang process?)

```
解决办法:
参考该链接即可:
[启动rabbitmq：ERROR: distribution port 25672 in use on localhost (by non-Erlang process?)](https://blog.csdn.net/silenceray/article/details/82655651)


参考资料如下:
[rabbitmq安装(centos7.8)](https://blog.csdn.net/jiguquan3839/article/details/91346261?utm_medium=distribute.pc_relevant_ask_down.none-task-blog-baidujs-3.nonecase&depth_1-utm_source=distribute.pc_relevant_ask_down.none-task-blog-baidujs-3.nonecase
)

[RabbitMQ 3.8.7 在 centos7 上安装](https://www.cnblogs.com/passerbywl/p/13617372.html)