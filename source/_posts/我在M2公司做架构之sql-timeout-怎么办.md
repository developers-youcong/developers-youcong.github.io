---
title: 我在M2公司做架构之sql timeout 怎么办
date: 2022-04-10 10:37:57
tags: ["架构","职业生涯","技术思考与实践"]
---

## 一、出现sql timeout有哪些情况?

核心错误信息如下:
```
### Cause: java.sql.SQLException: sql timeout
; uncategorized SQLException; SQL state [HY000]; error code [1023]; sql timeout; nested exception is java.sql.SQLException: sql timeout

```
<!--more-->

根据该错误信息，进行情况归纳，通常为如下三种原因:

- 1.连接数的限制；
- 2.后台大批量数据插入或更新，造成连接得不到释放；
- 3.mycat中间件处理问题。

## 二、如何解决sql timeout ?
- 1.上调最大连接数；
- 2.后台大批量数据插入是否真的有必要，如真的有必要，最好不要使用关系型数据库处理，可以使用NoSQL(例如MongoDB、HBase、Cassandra等)；
- 3.mycat中间件参数调整，对应的mysql可能需要调整。

基于上述三点即可解决该问题！！！