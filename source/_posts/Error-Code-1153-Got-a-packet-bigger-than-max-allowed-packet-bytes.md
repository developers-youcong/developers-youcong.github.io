---
title: 'Error Code: 1153 - Got a packet bigger than ''max_allowed_packet'' bytes'
date: 2021-02-06 19:48:59
tags: "Linux"
---

## 一、错误详细信息
```
Error occured at:2021-02-02 09:59:41
Line no.:87
Error Code: 1153 - Got a packet bigger than 'max_allowed_packet' bytes

```
<!--more-->
## 二、错误原因
导入的数据库脚本(xxx.sql)文件过大(超过几百M)，就会出现上面的错误。


## 三、解决办法

### 1.临时解决办法(MySQL一旦重启，配置就不生效了)
```
set global max_allowed_packet=524288000; 

```

### 2.全局解决办法(不管MySQL重启多少次，配置永久生效)
可通过修改my.ini文件(主要在my.ini添加如下内容):
```
max_allowed_packet=600M

```

修改完毕后，执行`/etc/init.d/mysql restart`命令即可。
