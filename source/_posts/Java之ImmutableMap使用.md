---
title: Java之ImmutableMap使用
date: 2022-01-16 11:50:59
tags: "Java"
---
需引入对应的Maven依赖:
```
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>${guava.version}</version>
</dependency>

```

可去Maven仓库搜索[guava](https://mvnrepository.com/artifact/com.google.guava/guava?__cf_chl_f_tk=_H2VIg65oZgMSZT_9b.n.QAIHX9ewvQUS_8NsGAnnms-1642306261-0-gaNycGzNCpE)即可！！！

ImmutableMap模板代码:
<!--more-->

```
# 默认创建时赋值
Map immutableMap = new ImmutableMap.Builder().put("A","A").put("B","B").build();
System.out.println("test:"+immutableMap.get("A"));

# 创建后不可变，会报错，错误关键信息为java.lang.UnsupportedOperationException
System.out.println("test:"+immutableMap.put("A","test"));

```

**应用场景归纳为如下几点：**

- 确定性配置(如第三方请求API地址、固定的业务性配置等)；
- 单元测试。

不适合key, value为未知参数, 因为可能有null产生的情况，也就代表key与value为确定值的场景均支持！！！