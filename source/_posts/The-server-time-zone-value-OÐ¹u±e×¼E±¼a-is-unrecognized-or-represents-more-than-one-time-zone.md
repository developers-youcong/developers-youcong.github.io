---
title: >-
  The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more
  than one time zone.
date: 2019-05-01 23:53:26
tags: "Java"
---

问题背景:
在Java中使用JDBC操作数据库，该数据库版本为8.0.15属于高版本(如果是低版本的话，通常是不会出现这些问题的)

详细错误信息如下:
```
java.sql.SQLException: The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.

```
<!--more-->
这个问题的原因是市区问题。

解决办法:
在jdbc对应的url加上serverTimezone=UTC即可解决，例如jdbc:mysql://localhost:3308/mysql?serverTimezone=UTC