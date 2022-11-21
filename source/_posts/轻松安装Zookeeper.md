---
title: 轻松安装Zookeeper
date: 2022-03-22 21:09:25
tags: "YC-Framework"
---

## 一、安装JDK
```
yum -y install java-1.8.0*

```
<!--more-->

## 二、安装Zookeeper

### 1.创建目录，自动下载安装包
```
#创建安装目录
mkdir -p /home/tech/zookeeper 
 
#移动到目录
cd /home/tech/zookeepe   
 
#下载zookeeper安装包
wget https://mirrors.aliyun.com/apache/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
 
#解压缩
tar -zxvf zookeeper-3.4.14.tar.gz

```

### 2.配置文件
```
#移到配置目录
cd /home/tech/zookeeper/zookeeper-3.4.14/conf/
 
#复制配置文件
cp zoo_sample.cfg zoo.cfg
 
#修改及添加以下配置
tickTime=2000 
initLimit=10
syncLimit=5
dataDir=/home/tech/zookeeper/zoodata
dataLogDir=/home/tech/zookeeper/zoodatalog
clientPort=2181
server.0=127.0.0.1:2888:3888
 
#多节点 集群
#server.1=127.0.0.1:4888:5888
#server.2=127.0.0.1:5888:6888
 
#保存退出
:wq或:wq!


```

配置说明:
tickTime：客户端会话超时时间，默认2000毫秒。

initLimit：配置客户端初始化可接受多少个心跳监测，默认10，即10*tickTime(默认2000)，表示20s没有连接上集群的配置则连接失败。

syncLimit：配置Leader和follwer之间，允许多少个请求应答长度，默认5，即5*tickTime(默认2000)，表示默认10sLeader和Follwer之间如果消息5次没有发送成功就不尝试了。

dataDir：配置存储快照文件的目录。

dataLogDir：配置事务日志存储的目录。

clientPort：服务默认端口，默认2181。

server.X=A:B:C 其中X是一个数字，表


### 3.创建节点
```
#创建dataDir目录
mkdir -p /home/tech/zookeeper/zoodata
 
#移动到目录
cd /home/tech/zookeeper/zoodata
 
#把节点号写入myid文件(各个节点分别配置)
echo 0 > myid
 
#配置端口防火墙(各个节点分别配置)
firewall-cmd --zone=public --add-port=2181/tcp --permanent
firewall-cmd --reload

```

### 4.启动Zookeeper
```
#移到执行目录
cd /opt/zookeeper/zookeeper-3.4.14/bin/
#启动服务
./zkServer.sh start

```

### 5.常用操作
```
#重启
./zkServer.sh restart
 
#关闭
./zkServer.sh stop
 
#查看状态
./zkServer.sh staus
 
#启动的时候，查看后台信息
./zkServer.sh start-foreground 

```