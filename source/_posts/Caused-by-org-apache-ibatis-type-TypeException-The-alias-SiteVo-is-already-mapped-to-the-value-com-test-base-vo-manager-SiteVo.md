---
title: >-
  Caused by: org.apache.ibatis.type.TypeException: The alias 'SiteVo' is already
  mapped to the value 'com.test.base.vo.manager.SiteVo'
date: 2020-11-05 19:30:34
tags: "MyBatis"
---

错误详细信息:
```
Caused by: org.apache.ibatis.type.TypeException: 
The alias 'SiteVo' is already mapped to the value 'com.test.base.vo.manager.SiteVo'

```
错误原因:
<!--more-->
关键在于配置文件指定别名范围过广，导致不同的包下出现相同的类，从而造成冲突显示上述的错误信息。

配置文件内容:
```
mybatis:
  # 搜索指定包别名
  typeAliasesPackage: com.test.**.entiy

```

解决办法1:
一般起名的话，建议最好不要起相同的。
改下相同类的名称即可。

解决办法2:
搜索指定包别名(上述配置文件)，最好指定确定的范围如com.test.blog.entity，这样就不会出现上面的错误。
