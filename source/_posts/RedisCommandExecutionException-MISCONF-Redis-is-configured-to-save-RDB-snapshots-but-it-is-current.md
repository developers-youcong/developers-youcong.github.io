---
title: >-
  RedisCommandExecutionException: MISCONF Redis is configured to save RDB
  snapshots, but it is current
date: 2021-01-11 21:09:29
tags: "Linux"
---

错误信息:
```
RedisCommandExecutionException: MISCONF Redis is configured to save RDB snapshots, but it is current

```

出现错误信息的原因:
还是因为授权，我的应用部署在/home下的某个用户目录下，而恰好其中一个应用在启动的时候会用到Redis进行数据初始化。初始化需要将MySQL的数据放到Redis中，而Redis则会将数据持久化，持久化涉及到存储，而存储势必会写入，因为Redis我放在/usr/software这个目录下，而我并未给这个用户授权，所以才导致上面的错误。

解决办法:
授权该用户有写入Redis的权限即可。

参考解决办法:
[解决Redis报错Redis is configured to save RDB snapshots, but it is currently not able to persist on disk](https://cloud.tencent.com/developer/article/1600527)