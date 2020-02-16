---
title: "Incorrect string value: 'è\x8E·å\x8F\x96...' for column 'result' at row 1"
date: 2019-10-16 15:59:23
tags: "MySQL"
---

错误详情信息:
```
### Error updating database.  Cause: java.sql.SQLException: Incorrect string value: '\xE8\x8E\xB7\xE5\x8F\x96...' for column 'result' at row 1
### The error may involve com.blog.springboot.dao.ApiLogDao.insert-Inline
### The error occurred while setting parameters
### SQL: INSERT INTO wp_api_log   ( class_name,  method_name,  params,  `result`,  consume_time,  update_date )  VALUES   ( ?,  ?,  ?,  ?,  ?,  ? )
### Cause: java.sql.SQLException: Incorrect string value: '\xE8\x8E\xB7\xE5\x8F\x96...' for column 'result' at row 1
; uncategorized SQLException for SQL []; SQL state [HY000]; error code [1366]; Incorrect string value: '\xE8\x8E\xB7\xE5\x8F\x96...' for column 'result' at row 1; nested exception is java.sql.SQLException: Incorrect string value: '\xE8\x8E\xB7\xE5\x8F\x96...' for column 'result' at row 1] with root cause

java.sql.SQLException: Incorrect string value: '\xE8\x8E\xB7\xE5\x8F\x96...' for column 'result' at row 1

```
<!--more-->

错误原因分析:
插入的数据列中，如果是中文的话，则会出现上面的错误，如果是英文的话，就不会出现错误。
故推测与字段列编码有关系，果然查看表对应的字符编码发现都是latin，最后将其改为utf8即可解决。


如果想永久解决这样的问题，可参考该链接:https://www.cnblogs.com/youcong/p/9374498.html
上面的问题我很久之前就遇到过，所以那个时候就已经记录了解决的办法。