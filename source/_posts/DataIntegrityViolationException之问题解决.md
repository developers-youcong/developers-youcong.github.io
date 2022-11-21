---
title: DataIntegrityViolationException之问题解决
date: 2022-06-25 19:20:26
tags: "MyBatis"
---

**详细错误信息如下:**
<!--more-->

```
org.springframework.dao.DataIntegrityViolationException: Error attempting to get column 'type' from result set.  Cause: java.sql.SQLDataException: Cannot determine value type from string 'A'
; Cannot determine value type from string 'A'; nested exception is java.sql.SQLDataException: Cannot determine value type from string 'A'

```

**错误原因：**
数据表返回的字段与实体属性对应不上造成的，例如数据表字段是varchar类型，实体属性用int或Integer接收。

**解决办法：**
实体属性对应数据表对应的字段类型即可，只要按照此种办法执行，基本上可避免出现类似问题。