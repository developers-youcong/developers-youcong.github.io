---
title: 轻松安装MongoDB
date: 2022-03-22 19:07:10
tags: "YC-Framework"
---

本文主要内容如下:

- 1.下载安装包；
- 2.配置环境变量；
- 3.建立日志、数据文件、配置文件夹；
- 4.启动mongo；
- 5.连接。

<!--more-->

## 一、下载安装包
```
cd /home/tech
# 下载安装包
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-5.0.2.tgz
# 解压安装包
tar -zxvf mongodb-linux-x86_64-rhel70-5.0.2.tgz
# 改名字
mv mongodb-linux-x86_64-rhel70-5.0.2 mongodb
# 此时可以删除安装包
rm -rf mongodb-linux-x86_64-rhel70-5.0.2.tgz 

```

## 二、配置环境变量
```
vim /etc/profile

## 文件中写入
export MONGO_HOME=/home/tech/mongodb
export PATH=$PATH:$MONGO_HOME/bin;
## 然后按下shift+俩次 z 键，保存输入信息
# 更新
source /etc/profile

```

## 三、建立日志、数据文件、配置文件夹
```
# 在 /home/tech/mongodb 路径下创建，如果不是需要切换进入
cd /home/tech/mongodb
# 创建三个文件夹
mkdir logs data conf
# 进入conf文件夹（写全路径防止大家走错位置）
cd /home/tech/mongodb/conf
# 写配置文件信息
vim mongodb.conf
## 内容如下
port=27017 #端口
bind_ip=0.0.0.0 #默认是127.0.0.1
dbpath=/home/tech/mongodb/data #数据库存放
logpath=/home/tech/mongodb/logs/mongodb.log #日志文件
fork=true #设置后台运行
#auth=true #开启


```

## 四、启动mongo
```
 启动命令
./mongod --config /home/tech/mongodb/conf/mongodb.conf
# 此处.conf文件路径一定不能出错


```

## 五、连接
```
mongo

```