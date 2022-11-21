---
title: SpringCloud Gateway 启动报错问题解决
date: 2021-11-13 23:31:56
tags: "SpringCloud"
---

详细错误信息如下:
<!--more-->

```
org.springframework.context.ApplicationContextException: Unable to start web server; nested exception is org.springframework.context.ApplicationContextException: Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:156) ~[spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:544) ~[spring-context-5.2.5.RELEASE.jar:5.2.5.RELEASE]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:141) ~[spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:747) [spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:397) [spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:315) [spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1226) [spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1215) [spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	at com.yc.gateway.YcGateWayApplication.main(YcGateWayApplication.java:17) [classes/:na]
Caused by: org.springframework.context.ApplicationContextException: Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.getWebServerFactory(ServletWebServerApplicationContext.java:203) ~[spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.createWebServer(ServletWebServerApplicationContext.java:179) ~[spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:153) ~[spring-boot-2.2.6.RELEASE.jar:2.2.6.RELEASE]
	... 8 common frames omitted



```


核心错误信息如下:
```
Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean

```

经过一系列问题排查，最终锁定是包的问题，出问题的Maven依赖如下:
```
 		<dependency>
            <groupId>com.yc.framework</groupId>
            <artifactId>yc-common-core</artifactId>
        </dependency>

```

报错原因是因为该依赖中引入mvc包，而这个mvc包与SpringBoot本身依赖产生了冲突。

解决方案如下(排除mvc依赖就能解决):
```
      <dependency>
            <groupId>com.yc.framework</groupId>
            <artifactId>yc-common-core</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework</groupId>
                    <artifactId>spring-webmvc</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

```