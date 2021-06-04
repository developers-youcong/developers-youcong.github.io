---
title: SpringCloud Alibaba之Nacos
date: 2020-08-23 10:06:26
tags: "SpringCloud"
---

## 一、运行Nacos
<!--more-->
Nacos GitHub开源地址:
https://github.com/alibaba/nacos

Nacos 官方文档:
https://nacos.io/zh-cn/docs/quick-start.html

按照如下命令即可:
```
//克隆
git clone https://github.com/alibaba/nacos.git

//进入对应目录
cd nacos/

//打包
mvn -Prelease-nacos -Dmaven.test.skip=true clean install -U  

//查看对应目录
ls -al distribution/target/


// 进入打包成功生成的目录结构(我本地nacos是1.3.1,所以对应的$version就是1.3.1)
cd distribution/target/nacos-server-$version/nacos/bin

//运行(以我本地windows为例，如果是Linux的话，执行startup.sh脚本即可)
startup.cmd

```

## 二、修改配置文件将Nacos的分布式配置存储改为MySQL
进入对应的目录:
```
cd D:\GitHub-Project\project\nacos\distribution\conf
```

修改application.properties文件，增加如下内容(对应的sql脚本在同一目录下，名字叫nacos-mysql.sql):
```
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3389/nacos-config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=1234


```

## 三、重新执行运行步骤中的打包操作(一定要打包成功，如果是遇到之前成功，这次失败，可能是配置文件写错的缘故)
```
mvn -Prelease-nacos -Dmaven.test.skip=true clean install -U  

```

## 四、运行成功的效果图(默认用户名和密码均为nacos/nacos)
![](SpringCloud-Alibaba之Nacos/01.png)

## 五、Nacos和Eureka对比

### 1.配置中心对比
- Nacos支持且用起来简单，符合SpringBoot命名风格，支持动态刷新。
- Eureka不支持(需要集成额外的SpringCloud Config组件)

### 2.注册中心对比

#### (1)eureka
- 应用内/外:直接集成到应用中，依赖于应用自身完成服务的注册和发现
- ACP原则:遵循AP（可用性+分离容忍)原则，有较强的可用性，服务注册快，但牺牲了一定的一致性
- 版本迭代:目前已经不再进行升级
- 集成支持:只支持SpringCloud集成
- 访问协议:HTTP
- 雪崩保护:支持雪崩保护
- 界面:英文界面，不符合国人习惯
- 上手:容易

#### (2)nacos
- 应用内/外:属于外部应用，侵入性小
- ACP原则:通知遵循CP原则(一致性+分离容忍)和AP原则(可用性+分离容忍)
- 版本迭代:目前仍然进行版本迭代
- 集成支持:支持Dubbo、SpringCloud、K8S集成
- 访问协议:HTTP/动态DNS/UDP
- 雪崩保护:支持雪崩保护
- 界面:中文界面，符合国人习惯(可根据自己需求，中英文任意切换)
- 上手:极易，中文文档，案例，社区活跃

关于我为什么选择Nacos而不选择Eureka，一方面我们的微服务框架是基于SpringCloud Alibaba的，如果直接切换，整个微服务框架根基都会有很大的动摇;另外一方面，Nacos目前已集成的正是我们所需要的如分布式配置、集群、服务注册和发现等;最后一方面，Nacos目前比Eureka版本迭代确实要活跃的多。


本文参考资料:
[Nacos官方文档](https://nacos.io/zh-cn/docs/quick-start.html)

[consul、eureka、nacos对比](https://www.cnblogs.com/zhucww/p/11532770.html)

[nacos简介以及作为注册/配置中心与Eureka、apollo的选型比较](https://www.jianshu.com/p/afd7776a64c6)