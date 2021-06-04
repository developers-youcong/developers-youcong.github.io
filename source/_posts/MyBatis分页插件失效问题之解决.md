---
title: MyBatis分页插件失效问题之解决
date: 2021-01-22 22:41:58
tags: "MyBatis"
---

今天遇到一个问题，MyBatis分页插件失效，导致分页无效，分页失效的原因是我在Controller里做了分页，但业务逻辑实现类对应的方法却写了两个SQL，第一个SQL是查询一条数据判断，第二个SQL是查询列表(是需要分页的)，结果通过日志打印SQL，我发现它却对第一个SQL做分页。最后我的解决办法是，在业务逻辑实现类对应的方法做分页，解决了这个问题。
<!--more-->

MyBatis实现分页很简单，需要引入如下两个依赖(以SpringBoot+MyBatis为例):
```
       <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.6</version>
        </dependency>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.12</version>
        </dependency>

```

核心模板代码:
```
 PageHelper.startPage(pageNum,pageSize);
 List<T> list = xxxService.getAll();
 PageInfo<T> pageInfo = new PageInfo<>(list);

```

通常建议这段代码写入Service层，最好不要写在Controller里面。