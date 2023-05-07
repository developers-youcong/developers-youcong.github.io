---
title: 从零开始学YC-Framework之TiDB
date: 2022-11-30 17:48:42
tags: "YC-Framework"
---

## 一、TiDB是什么？
<!--more-->
TiDB 是 PingCAP 公司自主设计、研发的开源分布式关系型数据库，是一款同时支持在线事务处理与在线分析处理 (Hybrid Transactional and Analytical Processing, HTAP) 的融合型分布式数据库产品，具备水平扩容或者缩容、金融级高可用、实时 HTAP、云原生的分布式数据库、兼容 MySQL 5.7 协议和 MySQL 生态等重要特性。目标是为用户提供一站式 OLTP (Online Transactional Processing)、OLAP (Online Analytical Processing)、HTAP 解决方案。TiDB 适合高可用、强一致要求较高、数据规模较大等各种应用场景。

## 二、TiDB具有哪些核心特性？
● 1.一键水平扩容或者缩容。
● 2.金融级高可用。
● 3.实时 HTAP。
● 4.云原生的分布式数据库。
● 5.兼容 MySQL 5.7 协议和 MySQL 生态。

## 三、TiDB具有哪些核心应用场景？
● 1.对数据一致性及高可靠、系统高可用、可扩展性、容灾要求较高的金融行业属性的场景。
● 2.对存储容量、可扩展性、并发要求较高的海量数据及高并发的 OLTP 场景。
● 3.Real-time HTAP 场景。
● 4.数据汇聚、二次加工处理的场景。

## 四、如何快速安装TiDB?

### 1.下载安装
```
curl --proto '=https' --tlsv1.2 -sSf https://tiup-mirrors.pingcap.com/install.sh | sh

```

安装完成后提示如下:
```
Successfully set mirror to https://tiup-mirrors.pingcap.com
Detected shell: bash
Shell profile:  /home/user/.bashrc
/home/user/.bashrc has been modified to add tiup to PATH
open a new terminal or source /home/user/.bashrc to use it
Installed path: /home/user/.tiup/bin/tiup
===============================================
Have a try:     tiup playground
===============================================

```
### 2.声明全局环境变量
```
source ${your_shell_profile}

```

### 3.启动
```
tiup playground

```

### 4.使用客户端连接
```
tiup client

```
同样也可以基于MySQL客户端连接:
```
mysql --host 127.0.0.1 --port 4000 -u root

```


## 五、有关TiDB相关的学习资料有哪些？
TiDB官网:
https://cn.pingcap.com/

TiDB源代码:
https://github.com/pingcap/tidb

TiDB文档:
https://docs.pingcap.com/zh/

TiDB快速安装:
https://docs.pingcap.com/zh/tidb/stable/quick-start-with-tidb

## 六、YC-Framework如何使用TiDB?
就跟使用MySQL配置一样。只需引入如下依赖即可:
```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

```

对应的配置文件，配置如下即可:
```
spring:
# 配置数据源
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:4000/yc-framework?useUnicode=true&characterEncoding=utf-8&&useOldAliasMetadataBehavior=true&useSSL=false
    username: xxxx
    password: xxxx

```

源代码均已开源，开源不易，如果对你有帮助，不妨给个star！！！

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework