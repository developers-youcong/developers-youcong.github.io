---
title: Centos7 cache/buff过高处理方法
date: 2020-09-30 17:44:01
tags: "Linux"
---

核心三条命令，即可清理cache/buff:
```
echo 1 > /proc/sys/vm/drop_caches

echo 2 > /proc/sys/vm/drop_caches

echo 3 > /proc/sys/vm/drop_caches

```
<!--more-->
参考资料:
[Centos7 cache/buff过高处理方法](https://blog.51cto.com/13578154/2150303?source=dra)