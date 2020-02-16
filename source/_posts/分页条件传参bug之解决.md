---
title: 分页条件传参bug之解决
date: 2019-12-04 22:25:37
tags: "Java"
---

问题描述:以对象作为参数，对象中包含PageNum、PageSize、Condition对象等。对应的@RequestBody为如PageReqDTO<T> reqDTO时，如果使用postman时，不在body中指定如下:
```
{"pageNum":1,"pageSize":10,"condition":{}}

```
<!--more-->
而是这样
```
{"pageNum":1,"pageSize":10}

```

就可能出现拿不到数据，拿不到数据又分为两种情况:

一种是传递对象没有做判断，例如没有在动态sql中`<if test="reqDTO!=null"></if>`，而是直接`<if test="reqDTO.value!='' and reqDTO.value!=null"></if>`

可能会报如下错误:
```
Caused by: org.apache.ibatis.ognl.OgnlException: source is null for getProperty(null, "userName")
mybatis


```

另外一种就是参数不对应，这种情况不是特别多，通常控制台也会报错，很容易解决这个错误。