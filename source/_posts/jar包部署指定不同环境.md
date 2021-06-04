---
title: jar包部署指定不同环境
date: 2020-10-20 21:51:29
tags: "Linux"
---

核心命令如下:
```
java -jar xxx.jar --spring.profiles.active=prod

```
<!--more-->
通过--spring.profiles.active指定不同的环境(如开发、测试、生产等)。

这非常重要因为涉及到部署脚本的编写。
