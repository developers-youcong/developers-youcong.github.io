---
title: >-
  net.sf.jsqlparser.statement.select.PlainSelect.getGroupByColumnReferences()Ljava/util/List(版本问题)
date: 2021-01-04 19:05:21
tags: "Java"
---

详细错误信息:
```
  net.sf.jsqlparser.statement.select.PlainSelect.getGroupByColumnReferences()Ljava/util/List

```

这个问题导致我的pagehelper分页失效出不来以及报错。

之前遇到过这样的错误，原因是因为pagehelper插件冲突导致的，我在这篇文章提到过[net.sf.jsqlparser.statement.select.PlainSelect.getGroupByColumnReferences()Ljava/util/List;](https://developers-youcong.github.io/2020/09/30/net-sf-jsqlparser-statement-select-PlainSelect-getGroupByColumnReferences-Ljava-util-List/)

这个错误原因是因为jsqlparser版本过高导致的。
<!--more-->

我原来的版本是3.1，如下所示:
```
<dependency>
<groupId>com.github.jsqlparser</groupId>
<artifactId>jsqlparser</artifactId>
<version>3.1</version>
</dependency>

```

现在改为1.4版本就好了，如下:
```
<dependency>
<groupId>com.github.jsqlparser</groupId>
<artifactId>jsqlparser</artifactId>
<version>1.4</version>
</dependency>

```

这个问题非常普遍，总时不时会冒出来，要从根本上解决这个问题，需要从研发规范方面入手，核心一句**就是不得随意引入其它得库，如果要引入，需做多次测试(兼容性)，确保不会导致项目的启动、接口开发、打包发布报错等。**

参考解决办法链接:
https://blog.csdn.net/qq_44804469/article/details/105906831