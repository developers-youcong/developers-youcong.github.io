---
title: >-
  Consider defining a bean of type
  'org.springframework.http.codec.ServerCodecConfigurer' in your configuration.
date: 2020-11-06 19:47:34
tags: "SpringBoot"
---

错误详细信息:
```
***************************
APPLICATION FAILED TO START
***************************

Description:

Parameter 1 of constructor in com.alibaba.cloud.sentinel.gateway.scg.SentinelSCGAutoConfiguration required a bean of type 'org.springframework.http.codec.ServerCodecConfigurer' that could not be found.


Action:

Consider defining a bean of type 'org.springframework.http.codec.ServerCodecConfigurer' in your configuration.


```
<!--more-->

#### 错误背景
集成redisson出现这个错误

redisson的maven依赖如下:
```
 <dependency>
    <groupId>org.redisson</groupId>
    <artifactId>redisson-spring-boot-starter</artifactId>
    <version>3.11.4</version>
 </dependency>

```

#### 错误原因分析
这是因为redisson里有spring-boot-starter-web导致的。因为我的springcloud-gateway也有这个依赖，依赖冲突导致启动报错。

#### 解决办法
排除依赖即可，如下:
```
  <dependency>
        <groupId>org.redisson</groupId>
        <artifactId>redisson-spring-boot-starter</artifactId>
        <version>3.11.4</version>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-webflux</artifactId>
            </exclusion>
        </exclusions>
   </dependency>

```

参考解决办法:
[springboot集成springCloud中gateway时启动报错](https://blog.csdn.net/github_38924695/article/details/99650634?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param)