---
title: 轻松安装RabbitMQ
date: 2022-03-22 20:46:32
tags: "YC-Framework"
---

轻松安装RabbitMQ，步骤如下:
<!--more-->

```
# 解压安装 xz
xz -d rabbitmq-server-generic-unix-3.9.8.tar.xz

# 解压安装 tar
tar -xvf rabbitmq-server-generic-unix-3.9.8.tar

# 重命名 rabbitmq
mv rabbitmq_server-3.9.8/ rabbitmq

# 配置环境变量
vim /etc/profile

编辑内容如下:
export RABBITMQ=/usr/local/rabbitmq
export PATH=$PATH:${RABBITMQ}/sbin

//刷新环境变量
source /etc/profile


//安装rabbitmq
# 安装页面管理插件
rabbitmq-plugins enable rabbitmq_management

# 开启服务，后台运行
rabbitmq-server -detached

# 注：添加用户和权限都要，先开启 RabbitMQ 服务
# 页面管理，用户 guest 是不能使用的，手动创建一个用户，并赋予权限
rabbitmqctl add_user admin admin

# 添加权限 .* 表示最高权限/所有权限
rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"

# 添加用户名角色，这里添加为 administrator (系统管理员)
rabbitmqctl set_user_tags admin administrator

# 综上修改，必须重启
rabbitmq-server restart

```