---
title: 'redis集群报错:(error) CLUSTERDOWN Hash slot not served'
date: 2020-09-22 19:29:37
tags: "Linux"
---

错误关键信息:
```
(error) CLUSTERDOWN Hash slot not served

```
<!--more-->
错误原因:
没有分配槽，因为redis集群要分配16384个槽来储存数据，那么没有分配槽则报如上错误


解决办法:
```
Can I set the above configuration? (type 'yes' to accept): 

你需要输入yes,而并非缩写 y，因为玩linux的都习惯的会输入 y，但是这里不行，要全拼yes才可以。

就是这个错误引起的分配槽失败。

```
参考解决方案:
[redis集群报错:(error) CLUSTERDOWN Hash slot not served](https://www.cnblogs.com/hanguoqing/p/10411128.html)