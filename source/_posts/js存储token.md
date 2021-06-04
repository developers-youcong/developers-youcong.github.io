---
title: js存储token
date: 2020-08-29 22:04:15
tags: "javascript"
---

关于存储token有多种方式(针对前端而言，如react可以使用redux存储token，js可以使用cookie存储token,还有今天说的通过sessionStorage保存token等)。

<!--more-->

sessionStorage相关操作核心代码如下:
```

sessionStorage.setItem("key","value"); //存储数据

sessionStorage.getItem("key"); //获取数据

sessionStorage.removeItem("key"); //删除数据

sessionStorage.clear(); //清除所有数据     


```