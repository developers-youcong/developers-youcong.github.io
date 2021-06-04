---
title: There is no normal TM
date: 2020-09-22 19:52:57
tags: "分布式事务"
---

错误关键信息:
```
There is no normal TM

```

只需在配置文件添加如下代码即可(application.properties):
```
tx-lcn.manager.host=0.0.0.0

```

错误原因:
与redis没有开放远程连接问题性质一样。

参考资料:
[记录一次Tx_LCN连接失败的问题( There is no normal TM )](https://blog.csdn.net/qq_43371556/article/details/105757288)