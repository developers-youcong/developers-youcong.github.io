---
title: 记一次layui弹框关闭问题
date: 2019-04-03 21:19:05
tags: "javascript"
---
我在博客园记录过layui关于弹框关闭问题，文章为[layui关闭弹出层](https://www.cnblogs.com/youcong/p/10371988.html)，这次出现了特殊情况，之前是通过`layer.closeAll()`解决了这个问题，但是这次解决不了。
而换成`parent.layer.closeAll()`问题就迎刃而解。
<!--more-->