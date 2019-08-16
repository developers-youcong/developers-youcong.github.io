---
title: MySQL之replace函数应用
date: 2019-07-31 15:41:46
tags: "MySQL"
---

replace函数，从字面上看其主要作用就是替换。实际它的作用确实是替换。
那么替换有哪些应用场景呢？
比如A表和B表有一个关联的字段就是id，但是在A中id是数字，在B中id也是数字，但是B中id多一个前缀字母t等，那么如果我要想让他们关联该怎么办呢？
通过replace就能实现这个目的，不用加字段或者强行修改让它们完全一致。
<!--more-->
下面我们来看replace函数结构:
```
REPLACE(表字段,原字符,替换字符)

```

实际例子如下:
```
SELECT pro.problem_id,pro.title,pro.problem_id FROM privilege AS p LEFT JOIN problem AS pro ON(REPLACE(p.rightstr,'p','') = pro.problem_id) WHERE p.user_id =#{userId}

```
