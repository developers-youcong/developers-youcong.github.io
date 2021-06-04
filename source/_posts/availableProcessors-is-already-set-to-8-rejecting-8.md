---
title: 'availableProcessors is already set to [8], rejecting [8]'
date: 2020-05-06 23:42:55
tags: "SpringBoot"
---

错误详细信息:
```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'elasticsearchClient' defined in class path resource [org/springframework/boot/autoconfigure/data/elasticsearch/ElasticsearchAutoConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.elasticsearch.client.transport.TransportClient]: Factory method 'elasticsearchClient' threw exception; nested exception is java.lang.IllegalStateException: availableProcessors is already set to [8], rejecting [8]

```
<!--more-->

关键信息如下:
```
Caused by: java.lang.IllegalStateException: availableProcessors is already set to [8], rejecting [8]
```

通过关键字搜索，找到了解决办法，**原因是因为SpringBoot的netty和elasticsearch的netty相关jar冲突**


只需在启动类加入如下代码即可解决(注意，这段代码要放在SpringApplication.run(Application.class, args)之前才行，否则不会生效):
```
System.setProperty("es.set.netty.runtime.available.processors", "false");

```