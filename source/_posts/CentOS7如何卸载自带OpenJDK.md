---
title: CentOS7如何卸载自带OpenJDK
date: 2021-05-16 11:56:23
tags: "Linux"
---
开发版的CentOS7.x一般自带OpenJDK,通常我们用商业版的Oracle JDK，这就需要将CentOS7.x自带的OpenJDK进行卸载。那么该如何卸载呢？

<!--more-->

## 1.首先，通过命令获取默认安装的OpenJDK
```
rpm -qa | grep java

```

## 2.其次，执行删除1中列出的jdk
```
rpm -e --nodeps 需要删除的OpenJDK名称

```
例如:
```
rpm -e --nodeps java-1.8.0-openjdk-1.8.0.191.b12-0.el7_5.x86_64

```