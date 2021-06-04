---
title: centos7通过yum安装jdk8
date: 2020-09-18 22:34:22
tags: "Linux"
---
## 一、查看系统是否已有自带的jdk
```
rpm -qa |grep java

rpm -qa |grep jdk

rpm -qa |grep gcj

```
<!--more-->
如果没有输出信息，则说明系统没有安装。如果有输出信息，则执行下面的命令卸载
```
rpm -qa | grep java | xargs rpm -e --nodeps

```

## 二、列出所有可安装的rpm软件包
```
yum list java-1.8*

```

## 三、安装jdk8
```
yum install java-1.8.0-openjdk* -y

```

验证安装是否成功:
```
java -version

```

参考链接:
[Centos7通过yum安装jdk8](https://my.oschina.net/u/3455207/blog/1858639)
