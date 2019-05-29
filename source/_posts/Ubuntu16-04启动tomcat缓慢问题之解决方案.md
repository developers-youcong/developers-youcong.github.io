---
title: Ubuntu16.04启动tomcat缓慢问题之解决方案
date: 2019-05-16 10:48:18
tags: "Linux"
---
问题信息:
```
16-May-2019 10:41:10.630 WARNING [localhost-startStop-1] org.apache.catalina.util.SessionIdGeneratorBase.createSecureRandom Creation of SecureRandom instance for session ID generation using [SHA1PRNG] took [117,835] milliseconds.

```

问题描述:
去官网下载tomcat后，解压本地并启动，发现启动极其缓慢，启动一个tomcat居然要十几分钟或者是始终启动不起来。
<!--more-->

问题原因:
是因为Tomcat8熵池阻塞变慢


解决方案:

(1)找到java.security文件(请执行该命令:cd /usr/lib/jvm/java-1.8.0-openjdk-amd64/jre/lib/security)

(2)编辑该文件(vim java.security)

将文件中的
```
securerandom.source=file:/dev/random

```
改为
```
securerandom.source=file:/dev/.urandom

```


参考问题解决链接:
[tomcat启动很慢 停留在 At least one JAR was scanned for TLDs yet contained no TLDs.](http://www.cnblogs.com/thinkingandworkinghard/p/6729705.html)
