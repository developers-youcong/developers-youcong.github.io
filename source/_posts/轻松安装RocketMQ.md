---
title: 轻松安装RocketMQ
date: 2022-03-22 20:56:38
tags: "YC-Framework"
---

## 一、下载
<!--more-->

```
wget https://archive.apache.org/dist/rocketmq/4.9.2/rocketmq-all-4.9.2-bin-release.zip

```

## 二、解压
```
unzip rocketmq-all-4.9.2-bin-release.zip

```

## 三、启动

### 1.启动name server
```
nohup sh bin/mqnamesrv & 
tail -f ~/logs/rocketmqlogs/namesrv.log

```

### 2.启动broker
```
nohup sh bin/mqbroker -n localhost:9876 & 
tail -f ~/logs/rocketmqlogs/broker.log

```

## 四、测试

### 1.发送消息
```
export NAMESRV_ADDR=localhost:9876 
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer


```

### 2.接收消息
```
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer


```

## 五、关闭server
```
sh bin/mqshutdown broker
sh bin/mqshutdown namesrv

```