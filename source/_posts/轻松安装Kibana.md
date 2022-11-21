---
title: 轻松安装Kibana
date: 2022-03-22 21:01:26
tags: "YC-Framework"
---

## 一、官网下载
```
https://www.elastic.co/cn/downloads/kibana

```
<!--more-->

## 二、解压
```
cd /home/tech/
tar -xzf kibana-7.4.0-linux-x86_64.tar.gz


```

## 三、配置
```
# vi /home/tech/kibana-7.4.0-linux-x86_64/config/kibana.yml
# 访问端口
server.port: 5601
# 表示可通过外网访问
server.host: "0.0.0.0"
# kibana服务名
server.name: "kibana-itcast"
# ES 地址
elasticsearch.hosts: ["http://127.0.0.1:9200"]
# ES 请求超时时间（默认30000ms）
elasticsearch.requestTimeout: 99999

```

## 四、启动
```
# 切换到kibana的bin目录
cd /home/tech/kibana-7.4.0-linux-x86_64/bin
# 启动
./kibana --allow-root
# 后台启动
nohup ./kibana &

```