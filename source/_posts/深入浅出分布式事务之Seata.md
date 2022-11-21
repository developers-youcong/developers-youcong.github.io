---
title: 深入浅出分布式事务之Seata
date: 2022-04-16 13:30:14
tags: ["架构","微服务","技术思考与实践","分布式","分布式事务"]
---

## 一、Seata是什么？
Seata 是一款开源的分布式事务解决方案，致力于在微服务架构下提供高性能和简单易用的分布式事务服务。在Seata开源之前，其内部版本在阿里经济体内部一直扮演着应用架构层数据一致性的中间件角色，帮助经济体平稳的度过历年的双11，对上层业务进行了有力的技术支撑。经过多年沉淀与积累，其商业化产品先后在阿里云、金融云上售卖。2019.1 为了打造更加完善的技术生态和普惠技术成果，Seata 正式宣布对外开源，未来 Seata 将以社区共建的形式帮助用户快速落地分布式事务解决方案。
<!--more-->

## 二、Seata常见术语有哪些？
- 1.TC (Transaction Coordinator) - 事务协调者(维护全局和分支事务的状态，驱动全局事务提交或回滚)。
- 2.TM (Transaction Manager) - 事务管理器(定义全局事务的范围：开始全局事务、提交或回滚全局事务)。
- 3.RM (Resource Manager) - 资源管理器(管理分支事务处理的资源，与TC交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚)。

## 三、Seata支持哪些服务注册中心呢？
- 1.Nacos；
- 2.Eureka；
- 3.Etcd3；
- 4.Consul；
- 5.Zookeeper。

## 四、Seata支持哪些ORM框架呢？
- 1.MyBatis；
- 2.MyBatis-Plus；
- 3.Spring Data JPA；
- 4.Hibernate。

## 五、数据库类型支持哪些呢(受分布式事务模式的影响，不同模式所支持的数据库类型不一样)？
- 1.MySQL；
- 2.Oracle；
- 3.PostgreSQL；
- 4.TiDB；
- 5.MariaDB。

## 六、分布式事务常见的模式有哪些？
- 1.AT模式；
- 2.TCC模式；
- 3.Saga模式；
- 4.XA模式。

## 七、关于Seata相关资料

Seata 官方文档:
https://seata.io/zh-cn/docs/overview/what-is-seata.html

Seata 博客:
https://seata.io/zh-cn/blog/index.html

Seata 源代码:
https://github.com/seata

Seata 代码样例:
https://github.com/seata/seata-samples

Seata 下载:
https://seata.io/zh-cn/blog/download.html