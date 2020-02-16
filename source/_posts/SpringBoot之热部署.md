---
title: SpringBoot之热部署
date: 2019-09-15 19:54:49
tags: "SpringBoot"
---


## 添加依赖

```
		<!--实现springboot的热加载-->
		<dependency>  
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
            <scope>true</scope>
        </dependency>


```


## application.yml添加对应的配置(这里以yml文件为例)
```
  devtools:
    restart:
      enabled: true
      additional-paths: src/main/java
      execlude: test/**

```

<!--more-->

接下来修改任意的Java代码，你会发现控制台自己会重新刷新一遍(修改对应的代码保存一下，控制台自动更新)

热部署主要方便开发人员调试程序，省得每次都要关闭再启动项目，提高开发和调试效率。