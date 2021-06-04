---
title: nacos的三种部署方式
date: 2020-09-05 15:07:39
tags: "Linux"
---

单机模式 - 用于测试和单机试用。
集群模式 - 用于生产环境，确保高可用。
多集群模式 - 用于多数据中心场景。

参考地址(参考官方文档即可):
https://nacos.io/zh-cn/docs/deployment.html
<!--more-->
我这边直接wget nacos微小版 然后执行如下命令，就实现了单部署:
```
wget https://developers-youcong.github.io/tags/https://github.com/alibaba/nacos/releases/download/1.3.0/nacos-server-1.3.0.zip

unzip nacos-server-1.3.0

cd nacos-server-1.3.0

cd bin
sh startup.sh -m standalone

```