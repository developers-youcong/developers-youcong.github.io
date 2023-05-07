---
title: 从零开始学YC-Framework之多数据源
date: 2022-11-28 20:59:54
tags: "YC-Framework"
---


## 一、什么是多数据源？
多数据源即在一个应用中配置多个不同的连接池(不同的连接池操作不同的数据库)。
<!--more-->

## 二、为什么要使用多数据源？
● 某特定业务场景下的需要(不同的公司，不同的业务场景)；
● 数据库拆分为多个，怎样让拆分后的数据库实现业务需要。

## 三、多数据源现成的方案有哪些？
● JdbcTemplate多数据源；
● MyBatis多数据源；
● MyBatis-Plus多数据源。

## 四、YC-Framework采用的多数据源方案是什么？
YC-Framework采用的是基于苞米豆生态中的dynamic-datasource。

**dynamic-datasource源代码:**
https://github.com/baomidou/dynamic-datasource-spring-boot-starter
**文档(文档大部分内容是付费的):**
https://www.kancloud.cn/tracy5546/dynamic-datasource/2264611

### 1.dynamic-datasource是什么？
dynamic-datasource-spring-boot-starter 是一个基于springboot的快速集成多数据源的启动器。

### 2.dynamic-datasource具有哪些特性？
● 支持 数据源分组 ，适用于多种场景 纯粹多库 读写分离 一主多从 混合模式。
● 支持数据库敏感配置信息 加密(可自定义) ENC()。
● 支持每个数据库独立初始化表结构schema和数据库database。
● 支持无数据源启动，支持懒加载数据源（需要的时候再创建连接）。
● 支持 自定义注解 ，需继承DS(3.2.0+)。
● 提供并简化对Druid，HikariCp，BeeCp,Dbcp2的快速集成。
● 提供对Mybatis-Plus，Quartz，ShardingJdbc，P6sy，Jndi等组件的集成方案。
● 提供 自定义数据源来源 方案（如全从数据库加载）。
● 提供项目启动后 动态增加移除数据源 方案。
● 提供Mybatis环境下的 纯读写分离 方案。
● 提供使用 spel动态参数 解析数据源方案。内置spel，session，header，支持自定义。
● 支持 多层数据源嵌套切换 。（ServiceA >>> ServiceB >>> ServiceC）。
● 提供 基于seata的分布式事务方案 。
● 提供 本地多数据源事务方案。
3.dynamic-datasource的约定是什么？
● 本框架只做 切换数据源 这件核心的事情，并不限制你的具体操作，切换了数据源可以做任何CRUD。
● 配置文件所有以下划线 _ 分割的数据源 首部 即为组的名称，相同组名称的数据源会放在一个组下。
● 切换数据源可以是组名，也可以是具体数据源名称。组名则切换时采用负载均衡算法切换。
● 默认的数据源名称为 master ，你可以通过 spring.datasource.dynamic.primary 修改。
● 方法上的注解优先于类上注解。
● DS支持继承抽象类上的DS，暂不支持继承接口上的DS。

### 4.如何使用dynamic-datasource?

#### (1)引入合适版本依赖
```
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
  <version>${version}</version>
</dependency>

```

#### (2)配置数据源(以YC-Framework使用druid作为多数据源连接池为例)
```
spring:
  application:
    # 应用名称
    name: yc-example-multi
  datasource:
    dynamic:
      primary: db1 #设置默认的数据源,默认值为master
      datasource:
        db1:  #数据源db1
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://127.0.0.1:3306/ycframework_master?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
          username: root
          password: 123456
        db2: #数据源db2
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://127.0.0.1:3306/ycframework_slave?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
          username: root
          password: 123456
      type: com.alibaba.druid.pool.DruidDataSource
      druid:
        initial-size: 10
        max-active: 100
        min-idle: 10
        max-wait: 60000
        pool-prepared-statements: true
        max-pool-prepared-statement-per-connection-size: 20
        time-between-eviction-runs-millis: 60000
        min-evictable-idle-time-millis: 300000
        #Oracle需要打开注释
        #validation-query: SELECT 1 FROM DUAL
        test-while-idle: true
        test-on-borrow: false
        test-on-return: false
        stat-view-servlet:
          enabled: true
          url-pattern: /druid/*
          #login-username: admin
          #login-password: admin
        filter:
          stat:
            log-slow-sql: true
            slow-sql-millis: 1000
            merge-sql: false
          wall:
            config:
              multi-statement-allow: true

```

#### (3)使用@DS切换数据源
@DS 可以注解在方法上或类上，同时存在就近原则 方法上注解 优先于 类上注解。

@DS("dsName")	dsName可以为组名也可以为具体某个库的名称。

如果没有@DS,则使用默认的数据源(见上述配置文件)。


相关示例代码地址:
https://github.com/developers-youcong/yc-framework/tree/main/yc-example/yc-example-multidatabase

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework
以上源代码均已开源，开源不易，如果对你有帮助，不妨给个star，鼓励一下！！！