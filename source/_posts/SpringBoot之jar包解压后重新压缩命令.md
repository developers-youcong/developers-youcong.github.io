---
title: SpringBoot之jar包解压后重新压缩命令
date: 2021-07-22 19:32:57
tags: "SpringBoot"
---

核心命令如下:
```
jar cvfM0 xxx.jar jar_path

```
<!--more-->

例如:
```
jar cvfM0 blog.jar *(将当前class文件打成jar包)

```
**
jar相关命令参数详解:**
-c  创建一个jar包
-t 显示jar中的内容列表
-x 解压jar包
-u 添加文件到jar包中
-f 指定jar包的文件名
-v  生成详细的报造，并输出至标准设备
-m 指定manifest.mf文件.(manifest.mf文件中可以对jar包及其中的内容作一些一设置)
-0 产生jar包时不对其中的内容进行压缩处理
-M 不产生所有文件的清单文件(Manifest.mf)。这个参数与忽略掉-m参数的设置
-i 为指定的jar文件创建索引文件
-C 表示转到相应的目录下执行jar命令,相当于cd到那个目录，然后不带-C执行jar命令

参考链接:
[jar命令的用法详解](https://www.cnblogs.com/liyanbin/p/6088458.html)