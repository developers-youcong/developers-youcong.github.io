---
title: springboot jar包部署如何不占用过多的服务器内存
date: 2020-09-05 15:45:46
tags: "Linux"
---

springboot部署方案有很多，可以将其打成war部署到tomcat，也可以直接jar部署(利用内嵌的tomcat),还可以使用docker部署等。

今天主要说的是springboot jar包部署占用内存确实有些大，该怎么调整呢？其实很简单，核心代码如下:
```
nohup java -Xms64m -Xmx128m -jar blog-gateway-2.0.0.jar  > blog-gateway.log 2>&1 &


```

最关键的是-Xms64m -Xmx128m，与JVM有关。

关于JVM内存设置可以参考如下链接:
https://www.cnblogs.com/jack204/archive/2012/07/02/2572932.html