---
title: >-
  BeanCreationException: Error creating bean with name
  'classPathFileSystemWatcher'之解决办法
date: 2019-11-30 19:43:36
tags: "SpringBoot"
---

错误关键信息:
```
BeanCreationException: Error creating bean with name 'classPathFileSystemWatcher'

```

错误原因:
Idea不支持热加载，application-test.yml中的热加载配置去除后，就能正常启动了，对应的服务也能正常访问。
<!--more-->

解决办法:
去除热加载中的代码配置，如可修改为这样:

```
   devtools:
      restart:
         enabled: true
#        additional-paths: src/main/java
         execlude: test/**

```