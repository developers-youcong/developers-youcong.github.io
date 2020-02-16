---
title: >-
  Command line is too long. Shorten command line for *** or also for Spring Boot
  default configuration
date: 2019-12-21 21:11:36
tags: "Idea"
---

错误信息:
```
Command line is too long. Shorten command line for *** or also for Spring Boot default configuration

```
通常会导致的后果是无法启动项目。

解决办法:

修改项目下 .idea\workspace.xml，找到标签 <component name="PropertiesComponent"> ， 在标签里加一行 
```
 <property name="dynamic.classpath" value="true" />

```

参考解决办法链接:https://blog.csdn.net/weixin_41235754/article/details/100514000
<!--more-->