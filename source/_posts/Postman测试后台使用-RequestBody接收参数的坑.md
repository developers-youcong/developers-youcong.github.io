---
title: Postman测试后台使用@RequestBody接收参数的坑
date: 2019-11-30 16:47:25
tags: "计算机"
---

问题原因:我在使用PostMan测试接口时发现数据传递不过来，是因为请求体定义为JSON数据，自动就传递不过来，虽然问题简单，但由于之前这个用的较少，所以就忽略了这点。

解决问题链接:https://blog.csdn.net/qq_24484085/article/details/82800798