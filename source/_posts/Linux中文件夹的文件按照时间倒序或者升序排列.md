---
title: Linux中文件夹的文件按照时间倒序或者升序排列
date: 2021-09-25 13:31:17
tags: "Linux"
---
应用场景:
查询服务器上某个特定文件夹下的文件定时最新生成情况。
<!--more-->

## 核心命令(升序)
```
命令:ls -lrt
详细解释:

-l     use a long listing format  以长列表方式显示（详细信息方式）
-t     sort by modification time 按修改时间排序（最新的在最前面）
-r     reverse order while sorting （反序）

```


## 核心命令(降序)
```
命令:ls -lt
详细解释:

-l     use a long listing format  以长列表方式显示（详细信息方式）
-t     sort by modification time 按修改时间排序（最新的在最前面）

```