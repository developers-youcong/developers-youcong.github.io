---
title: '[ERR] 1273 - Unknown collation: ''utf8mb4_0900_ai_ci'''
date: 2023-01-29 17:44:33
tags:
---

使用Navicate运行sql文件出错，错误信息如下:
<!--more-->
```
[ERR] 1273 - Unknown collation: 'utf8mb4_0900_ai_ci'

```
**报错原因：**
生成转储文件的数据库版本为8.0,要导入sql文件的数据库版本为5.6,因为是高版本导入到低版本，引起1273错误

**解决方法：**
打开sql文件，将文件中的所有
utf8mb4_0900_ai_ci替换为utf8_general_ci
utf8mb4替换为utf8
保存后再次运行sql文件，运行成功