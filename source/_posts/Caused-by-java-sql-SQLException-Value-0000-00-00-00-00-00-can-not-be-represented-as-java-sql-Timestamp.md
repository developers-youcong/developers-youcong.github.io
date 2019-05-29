---
title: >-
  Caused by: java.sql.SQLException: Value '0000-00-00 00:00:00' can not be
  represented as java.sql.Timestamp
date: 2019-04-07 14:36:58
tags: "MyBatis"
---
错误信息如下:
```
Caused by: java.sql.SQLException: Value '0000-00-00 00:00:00' can not be represented as java.sql.Timestamp
```
原因如下:
是因为数据表中字段类型与对象中的属性类型不一致。比如在我的数据表中是datetime类型，正常来说，对象中应该是Date类型，但是本次在对象中却是String类型。

解决办法:
(1)将datetime类型修改为varchar类型，即可解决问题;
(2)将Java对象属性类型(对应的那个)改为Date类型(java.util而非java.sql),即可解决问题;