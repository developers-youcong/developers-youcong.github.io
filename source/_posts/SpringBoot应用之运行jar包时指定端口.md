---
title: SpringBoot应用之运行jar包时指定端口
date: 2020-09-11 20:26:40
tags: "SpringBoot"
---

应用场景:
同一个jar项目可以在一台服务器多部署。

核心命令如下:
```
java -jar XXXXX.jar --server.port=8080

```

参考资料:
[运行jar包指定端口](https://blog.csdn.net/wangxiaofeng0010/article/details/78520018)