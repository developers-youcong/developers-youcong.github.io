---
title: Java Date实现+天之核心代码
date: 2020-03-15 00:29:40
tags: "Java"
---

reqDTO是传递对象，而getValidNum是具体的时间，默认为int类型,根据前台传递的数字进行天数相加。
<!--more-->
核心代码如下:
```
        Calendar c = Calendar.getInstance();
        c.setTime(new Date());
        c.add(Calendar.DAY_OF_MONTH, reqDTO.getValidNum());
        Date expireTime = c.getTime();

```