---
title: Linux下删除当前目录及子目录下的所有.txt文件
date: 2021-06-03 19:26:30
tags: "Linux"
---

因为某种应用场景我需要将特定目录下的txt文件进行清理，核心命令如下:
<!--more-->
```
find 目录 -name '*.txt' -type f -print -exec rm -rf {} \;

```
例如:
```
find /home/tech/data_log -name '*.txt' -type f -print -exec rm -rf {} \;

```
这样就能将data_log目录下的所有txt进行删除。当然了，不仅仅是txt文件，还可以是其它的文件。
