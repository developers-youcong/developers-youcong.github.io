---
title: js正则表达式之解决html解析<>标签问题
date: 2019-08-26 16:28:15
tags: "javascript"
---

应用场景:
以博客写文章为例，有的时候我们不经意间写的字符串带标签，然后浏览器将其解析了，实际上我们并不希望其被解析，于是可通过核心代码解决该问题。

核心代码如下:
```
data.codeSource.replace(new RegExp("<","g"),"&lt;").replace(new RegExp(">","g"),"&gt;")

```

data.codeSource在这里相当于与后台交互获取到的数据

new RegExp是js正则表达式对象

replace方法，一共有两个参数，一个是原来的字符串，另一个是新的字符串

replace(old_string,new_string)

<!--more-->

