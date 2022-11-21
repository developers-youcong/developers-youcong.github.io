---
title: Windows之Bat脚本获取指定文件夹下的所有文件全路径
date: 2022-07-16 14:14:42
tags: "计算机"
---

在指定文件夹下新建file.bat脚本，并添加如下内容:
<!--more-->

```
DIR *.*  /S/B >file.txt

```

双击执行file.bat文件，会在当前文件夹下生成file.txt文件，该文件中会有当前指定文件夹下的所有文件全路径。