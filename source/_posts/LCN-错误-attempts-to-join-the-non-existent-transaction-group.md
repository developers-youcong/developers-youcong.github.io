---
title: 'LCN 错误: attempts to join the non-existent transaction group'
date: 2020-09-22 19:49:13
tags: "分布式事务"
---

错误信息:
```
com.codingapi.txlcn.logger.AbstractTxLogger.error(AbstractTxLogger.java:70) - business code error

attempts to join the non-existent transaction group

rpc execute service error. action: joinGroup

```

解决办法:
在tx-lcn项目配置如下内容，即可解决:
```
# 分布式事务执行总时间(ms). 默认为36000
tx-lcn.manager.dtx-time=600000

```

配置完需要重新打包。

参考资料:
[LCN 错误: attempts to join the non-existent transaction group](https://blog.csdn.net/qq_41463655/article/details/106483312)