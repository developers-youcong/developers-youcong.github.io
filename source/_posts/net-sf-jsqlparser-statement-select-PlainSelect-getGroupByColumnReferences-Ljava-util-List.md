---
title: >-
  net.sf.jsqlparser.statement.select.PlainSelect.getGroupByColumnReferences()Ljava/util/List;
date: 2020-09-30 17:30:21
tags: "Java"
---

错误信息:
```
net.sf.jsqlparser.statement.select.PlainSelect.getGroupByColumnReferences()Ljava/util/List;

```

这个错误导致我启动项目失败。
<!--more-->

错误原因:
发现是pagehelper插件冲突导致的(我引入了一个pagehelper，同事又引入了一个pagehelper，版本不一样，去除她的就好了)。

搜了相关资料说是它的冲突:
```
<dependency>
<groupId>com.github.jsqlparser</groupId>
<artifactId>jsqlparser</artifactId>
<version>3.1</version>
</dependency>

```

但是我的项目并沒有引入它(估计可能是maven的依赖传递)。

参考资料:
[net.sf.jsqlparser.statement.select.PlainSelect.getGroupByColumnReferences()Ljava/util/List;](https://blog.csdn.net/qq_44804469/article/details/105906831)