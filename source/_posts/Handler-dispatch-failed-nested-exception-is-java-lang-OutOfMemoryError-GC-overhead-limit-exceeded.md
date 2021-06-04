---
title: >-
  Handler dispatch failed; nested exception is java.lang.OutOfMemoryError: GC
  overhead limit exceeded
date: 2020-09-26 12:39:07
tags: "java"
---

错误详细信息:
```

org.springframework.web.util.NestedServletException: Handler dispatch failed; nested exception is java.lang.OutOfMemoryError: GC overhead limit exceeded


```
<!--more-->
错误原因:
部署springboot微服务时，
java -Xms64m -Xmx128m -jar xxx.jar
分配内存过小导致。

解决方案:
设置大点或去除-Xms64m -Xmx128m ，就能解决这个问题。