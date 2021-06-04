---
title: Cannot create resource output directory
date: 2020-07-28 17:51:52
tags: "Maven"
---

错误背景:
mvn clean package下报错
<!--more-->
错误关键信息:
```
Cannot create resource output directory

```

错误原因:
有其它地方占用该资源。

解决办法:
关闭其它占用该资源的地方即可解决该问题。
