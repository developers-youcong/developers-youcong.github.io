---
title: >-
  org.springframework.context.ApplicationContextException: Unable to start web
  server; nested exceptio
date: 2020-09-05 15:13:12
tags: "SpringBoot"
---

详细错误信息:
```
org.springframework.context.ApplicationContextException: Unable to start web server; nested exception is org.springframework.context.ApplicationContextException: Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.


```

常规解决办法:
参考如下链接即可:
[org.springframework.context.ApplicationContextException: Unable to start web server; nested exceptio](https://cloud.tencent.com/developer/article/1536938)


但是也分情况。比分我这次遇到是关于不同Maven多模块相互依赖关系导致此类错误，理解Maven多模块之间的依赖关系即可解决。

由此可以看出Maven多模块一定得设计好以及依赖关系一定要明确，否则项目启动报错、打包报错等各种奇葩错误弄得你怀疑人生。

不过兵来将挡，水来土淹。经过这几年得错误洗礼，面对错误，我也不再像过去烦躁或胆怯，因为我知道烦躁和胆怯是解决不了问题的。面对错误，就是要直接面对它，然后解决它。我提供我的**通用思路**:
<!--more-->
**1.把握关键错误信息**

**2.根据错误信息推断可能是什么原因**

**3.要有详细复现步骤(页面操作，IDE Debug调试)**

**4.找到错误根本原因**

**5.最终解决它**

有的时候不能面面俱到做到这五步，因为项目非常紧急的情况下和任务繁重，直接把握好第一步，然后把关键错误信息放搜索引擎一搜基本上就能得到现成的解决方案。

当然了，这需要你具有甄别正确或错误的能力，不然的话，很容易被坑。
