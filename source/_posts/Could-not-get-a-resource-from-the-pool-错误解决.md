---
title: Could not get a resource from the pool 错误解决
date: 2019-03-09 20:26:18
tags: "Redis"
---
错误关键信息:Could not get a resource from the pool

通常原因是因为远程服务器上的redis没有配置好。

解决方案如下:
(1)将redis.conf中的bind:127.0.0.1注释掉;
(2)将redis.conf中的protected-mode yes改为protected-mode no

按照上述的解决方案是可以解决这个问题的。但是以SpringBoot为例，这样做仍然无法解决问题，原因是因为application.yml中的redis配置有误造成的。

<!--more-->

按照如下配置即可解决问题:
```
spring:
  redis:
    host: 192.168.126.128
    port: 2019
    password: youcongtech
    database: 0
    lettuce:
      pool:
        max-active: 32
        max-wait: 300ms
        max-idle: 16
        min-idle: 8
```

之所以这样配置是因为使用的是spring-boot-starter-data-redis这个maven依赖。当然了，如果你不想这样配置的话大可自己写一个Jedis，不过通常Maven已经提供了，不必自己动手造轮子。

详情可参考如下:
Java连接Redis之redis的增删改查:https://www.cnblogs.com/youcong/p/8098881.html

如果你还没有安装过Redis可以参考我的这篇文章[Redis的安装和客户端使用注意事项](https://www.cnblogs.com/youcong/p/8044625.html)