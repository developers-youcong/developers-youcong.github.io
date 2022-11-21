---
title: Java8 Stream判断List对象中是否包含对象的值
date: 2022-06-25 19:26:54
tags: "Java"
---

常用的办法以下两种：
<!--more-->

```
boolean result01 = dataList.stream().filter(m -> m.getType().equals("1")).findAny().isPresent();

boolean result02 = dataList.stream().anyMatch(m -> m.getType().equals("1"))


```

如果不用Java8 Stream特性，要么通过SQL一次性查出来获取结果；要么采用常规List遍历法判断来获取结果