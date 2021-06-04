---
title: closing inbound before receiving peer's close_notify
date: 2020-09-16 21:31:51
tags: "Java"
---

错误详细信息:
```
javax.net.ssl.SSLException: closing inbound before receiving peer's close_notify
    at java.base/sun.security.ssl.Alert.createSSLException(Alert.java:129)
    at java.base/sun.security.ssl.Alert.createSSLException(Alert.java:117)
    at java.base/sun.security.ssl.TransportContext.fatal(TransportContext.java:308)
    at java.base/sun.security.ssl.TransportContext.fatal(TransportContext.java:264)
    at java.base/sun.security.ssl.TransportContext.fatal(TransportContext.java:255)
    at java.base/sun.security.ssl.SSLSocketImpl.shutdownInput(SSLSocketImpl.java:645)
    at java.base/sun.security.ssl.SSLSocketImpl.shutdownInput(SSLSocketImpl.java:624)
    at com.mysql.cj.protocol.a.NativeProtocol.quit(NativeProtocol.java:1312)
    at com.mysql.cj.NativeSession.quit(NativeSession.java:182)
    at com.mysql.cj.jdbc.ConnectionImpl.realClose(ConnectionImpl.java:1750)
    at com.mysql.cj.jdbc.ConnectionImpl.close(ConnectionImpl.java:720)
    at com.zaxxer.hikari.pool.PoolBase.quietlyCloseConnection(PoolBase.java:135)
    at com.zaxxer.hikari.pool.HikariPool.lambda$closeConnection$1(HikariPool.java:441)
    at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
    at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
    at java.base/java.lang.Thread.run(Thread.java:834)

```
<!--more-->
解决问题办法:
只需在数据库连接URL加useSSL=false即可。

参考解决问题链接:
[closing inbound before receiving peer's close_notify的解决办法](https://blog.csdn.net/qq_36924683/article/details/90167484)