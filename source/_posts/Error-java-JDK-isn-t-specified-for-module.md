---
title: 'Error:java:JDK isn''t specified for module'
date: 2020-03-22 21:02:11
tags: "Idea"
---

错误信息如下:
```
Error:java:JDK isn't specified for module "XXXX"

```
<!--more-->
错误原因:项目中的.idea文件夹被删掉，导致项目目录出错

解决办法:
idea中关掉该项目并删除，然后重新import引入该项目，在弹出是否重写.idea选择是

参考解决办法链接:
https://blog.csdn.net/zzzU5U6/article/details/88951286