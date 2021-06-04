---
title: SpringCloud之Sentinel
date: 2020-11-07 14:50:36
tags: "SpringCloud"
---

## 一、Sentinel

Sentinel GitHub地址:
https://github.com/alibaba/Sentinel

关于Sentinel详细介绍:
https://github.com/alibaba/Sentinel/wiki/%E4%BB%8B%E7%BB%8D

Sentinel官方文档:
https://sentinelguard.io/zh-cn/docs/introduction.html
<!--more-->
这里只明确一点，Sentinel主要作用是**在高流量下如何保持服务的稳定性。**


## 二、下载运行Sentinel

### 1.下载源代码
```
git clone https://github.com/alibaba/Sentinel.git


```

### 2.打包
```
cd Sentinel

mvn clean package -Dmaven.test.skip=true

```
**特别注意:**
Sentinel项目路径不能在有中文，否则会报错。

### 3.打包成功指定jar运行
```
java -jar sentinel-dashboard.jar

```

Linux部署只需执行jar即可。
平时如果需要调试，只需导入Sentinel到IDE，然后直接运行DashboardApplication.java文件即可(sentinel-dashboard模块下)。

## 三、SpringCloud整合Sentinel

**注意:**
我的SpringCloud Version为Hoxton.SR4。

### 1.Maven依赖
父pom.xml
```
      
    <dependencyManagement>
        <dependencies>
            <!-- SpringCloud Alibaba 微服务 -->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.2.1.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

```

子pom.xml(将其依赖放入网关模块里的pom.xml即可):
```

        <!-- SpringCloud Ailibaba Sentinel -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>


        <!-- SpringCloud Ailibaba Sentinel Gateway -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-sentinel-gateway</artifactId>
        </dependency>

```

### 2.gateway模块配置文件(application.yml)，添加如下内容
```
spring:
  cloud:
    sentinel:
      # 取消控制台懒加载
      eager: true
      transport:
        # 控制台地址
        dashboard: 127.0.0.1:8718

```

我的详细配置文件内容如下:
```
server:
  port: 8080

spring:
  cloud:
    sentinel:
      # 取消控制台懒加载
      eager: true
      transport:
        # 控制台地址
        dashboard: 127.0.0.1:8718
    gateway:
      discovery:
        locator:
          lowerCaseServiceId: true
          enabled: true
      routes:
        # 第三方API
        - id: blog-api
          uri: lb://blog-api
          predicates:
            - Path=/api/**
          filters:
            - StripPrefix=1
        ## 后台
        - id: blog-admin
          uri: lb://blog-admin
          predicates:
            - Path=/admin/**
          filters:
            - StripPrefix=1
        ## PC端
        - id: blog-portal
          uri: lb://blog-portal
          predicates:
            - Path=/portal/**
          filters:
            - StripPrefix=1
        ## 工具
        - id: blog-tools
          uri: lb://blog-tools
          predicates:
            - Path=/blog-tools/**
          filters:
            - StripPrefix=1

application:
  name: blog-gateway-server

eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/

```

### 3.启动项目

启动项目不报错，然后访问Sentinel，有如下效果，表示整合成功:
![图一](SpringCloud之Sentinel/01.png)

接着访问网关下的blog-api的接口，Sentinel实时监控效果如下:
![图二](SpringCloud之Sentinel/02.png)

### 4.说明
Sentinel功能很强大，这里列举仅仅是整合，如用得比较多需深入学习。