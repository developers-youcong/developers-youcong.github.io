---
title: 我在M2公司做架构之Redis
date: 2021-10-31 10:33:19
tags: ["架构","职业生涯","技术思考与实践"]
---

## 一、Redis的定义
Redis是现在最受欢迎的NoSQL数据库之一，Redis是一个使用ANSI C编写的开源、包含多种数据结构、支持网络、基于内存、可选持久性的键值对存储数据库，其具备如下特性：
<!--more-->

- 基于内存运行，性能高效；
- 支持分布式，理论上可以无限扩展；
- key-value存储系统；
- 开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

## 二、和其它数据库相比，Redis具备哪些特点
- C/S通讯模型；
- 单进程单线程模型；
- 丰富的数据类型；
- 操作具有原子性；
- 持久化；
- 高并发读写；
- 支持lua脚本。

## 三、国内外有哪些大厂在使用Redis
- GitHub；
- Twitter；
- StackoverFlow；
- 阿里巴巴；
- 百度；
- 美团；
- 微博；
- 腾讯；
- 搜狐；
- 当当；
- 滴滴；
- 字节跳动。

不仅仅是上面的十二家公司还有更多公司都在使用Redis。


## 四、Redis的应用场景有哪些
- 缓存热点数据；
- 计数器(例如统计访问量)；
- 限流器(例如限制某个ip频繁访问或者是超出请求最大限制数拒绝访问)；
- 发布-订阅；
- 排行榜；
- 分布式锁；
- 分布式会话；
- 消息队列；
- 社交网络(点赞、关注/被关注、共同好友等)。

## 五、Redis在架构中主要做哪些事情
由于我们是分布式架构，既包含分布式相关，如分布式锁、分布式会话，也包含其它的，例如缓存热点数据、特定业务数据存储、计数器等。

## 六、我以往写的关于Redis的博客文章
[SpringBoot-MyBatis-Redis-二级缓存](https://youcongtech.com/2020/09/11/SpringBoot-MyBatis-Redis-%E4%BA%8C%E7%BA%A7%E7%BC%93%E5%AD%98/)

[SpringBoot整合Redisson-单机版](https://youcongtech.com/2020/11/06/SpringBoot%E6%95%B4%E5%90%88Redisson-%E5%8D%95%E6%9C%BA%E7%89%88/)

[SpringBoot整合Redisson-集群版](https://youcongtech.com/2020/11/06/SpringBoot%E6%95%B4%E5%90%88Redisson-%E9%9B%86%E7%BE%A4%E7%89%88/)

[Redis集群搭建](https://youcongtech.com/2020/09/22/redis%E9%9B%86%E7%BE%A4%E6%90%AD%E5%BB%BA/)

[SpringBoot集成Redis](https://segmentfault.com/a/1190000038170500)

[redis集群之节点少于六个错误-解决](https://www.cnblogs.com/youcong/p/13734596.html)

[redis集群报错:(error) CLUSTERDOWN Hash slot not served](https://www.cnblogs.com/youcong/p/13734582.html)

[Redis口令设置](https://www.cnblogs.com/youcong/p/9664280.html)

[Java连接Redis之redis的增删改查](https://www.cnblogs.com/youcong/p/8098881.html)

[Redis启动问题解决方案](https://www.cnblogs.com/youcong/p/9664257.html)

[Redis简单集群配置](https://www.cnblogs.com/youcong/p/9409522.html)

[Redis的安装和客户端使用注意事项](https://www.cnblogs.com/youcong/p/8044625.html)

[RedisCommandExecutionException: MISCONF Redis is configured to save RDB snapshots, but it is current](https://www.cnblogs.com/youcong/p/14274154.html)