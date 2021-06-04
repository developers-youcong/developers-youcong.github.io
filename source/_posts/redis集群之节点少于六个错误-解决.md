---
title: redis集群之节点少于六个错误-解决
date: 2020-09-22 19:57:17
tags: "Linux"
---

错误详细信息:
```
*** ERROR: Invalid configuration for cluster creation.
*** Redis Cluster requires at least 3 master nodes.
*** This is not possible with 4 nodes and 1 replicas per node.
*** At least 6 nodes are required.

```
解决方案:
增加节点即可。

参考资料:
[当节点数量少于6个时候会提示如下信息,初始化一个集群的时候需要6个节点，为什么??](https://www.cnblogs.com/linyilong3/p/6033901.html)