---
title: 轻松安装ElasticSearch
date: 2022-04-04 16:00:19
tags: "YC-Framework"
---

前提:
需要安装JDK，关于安装JDK可以参考我如下文章:
[centos7通过yum安装jdk8](https://www.cnblogs.com/youcong/p/13693933.html)
<!--more-->

## 一、上传elastic-search压缩包至Linux服务器(工具可以是git bash 也可以是xftp或winscp等)

## 二、解压
```
tar -xzvf elasticsearch-7.4.0-linux-x86_64.tar.gz

```

## 三、启动
```
cd /home/tech/elasticsearch-7.4.0
./elasticsearch -d

```
