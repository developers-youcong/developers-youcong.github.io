---
title: attempted to return null from a method with a primitive return type (int).
date: 2020-02-28 22:32:12
tags: "MyBatis"
---

错误信息:
```
attempted to return null from a method with a primitive return type (int).
```

错误原因:
实际查询sql并没有这个值，出现空值，就会报这个异常。
<!--more-->

错误sql(发生问题):
```
select sum(id) from post where post.author = ?

```


正确sql(解决问题):
```
select IFNULL(sum(id),0) from post where post.author = ?
```

关键在于使用了IFNULL，有了这个判断可避免任何情况空值导致的错误。