---
title: Nacos安全配置之思考
date: 2022-04-04 17:42:38
tags: ["分布式","中间件","技术思考与实践"]
---

最近基于Nacos安全配置相关的实践落地，总结了三个方面的措施，分别如下:

- 1.基于Nacos与微服务；
- 2.基于Nacos本身；
- 3.服务器层面。

<!--more-->

## 一、基于Nacos与微服务

### 1.编辑Nacos的配置文件(application.properties)
将如下由原来的false改为true：
```
nacos.core.auth.caching.enabled=true

```

### 2.对应的微服务配置
bootstrap.yml或application.yml:
```
cloud:
    nacos:
      username: nacos_user
      password: nacos_pwd
      discovery:
        # 服务注册地址
        server-addr: 127.0.0.1:8848
      config:
        # 配置中心地址
        server-addr: 127.0.0.1:8848
        # 配置文件格式
        file-extension: yml
        # 共享配置
        shared-dataids: application-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}
        refresh-enabled: true
        refreshable-dataids: application-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}

```

通过上面两步就能实现Nacos认证(没有正确用户名和密码，仅知道IP和端口是无法连接，从应用层面入手防止一些非法连接操作，从而增加Nacos与微服务通信过程的安全性)。


## 二、基于Nacos本身
- 1.Nacos使用代理，且代理通过SSL处理；
- 2.关注Nacos最新信息(每个版本的迭代情况，着重于漏洞方面)；
- 3.定期更换Nacos密码；
- 4.开启权限认证(前面提到过)。


## 三、服务器层面
可以阅读我之前写的[服务器安全策略之思考与实践](https://youcongtech.com/2021/07/16/%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%89%E5%85%A8%E7%AD%96%E7%95%A5%E4%B9%8B%E6%80%9D%E8%80%83%E4%B8%8E%E5%AE%9E%E8%B7%B5/)这篇文章。