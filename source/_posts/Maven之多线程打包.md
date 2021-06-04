---
title: Maven之多线程打包
date: 2020-12-12 21:05:48
tags: "Maven"
---

Maven多线程打包核心命令:
```
mvn clean package -T 1C -Dmaven.test.skip=true  -Dmaven.compile.fork=true

```

主要解决多模块编译打包速度问题。