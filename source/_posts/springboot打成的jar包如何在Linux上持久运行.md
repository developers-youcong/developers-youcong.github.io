---
title: springboot打成的jar包如何在Linux上持久运行
date: 2019-02-23 17:16:10
tags: "SpringBoot"
---

## 一、首先说说在没有springboot的时候，项目是如何部署的？

#### 1.动态web项目

动态web项目部署很方便，基本上上传文件到服务器的tomcat里面的webapps文件夹下即可完成部署。
当然了，这种做法的弊端是，如果是通过winscp来传输对于网速方面要求严格，不然的话网速一卡，很久传不过去，很耽误时间的，当然了，我一个同学他们公司用的就是动态web项目，部署的方式也正是采用这种方式，据说是公司制度定下的。原因我就没有细问过他。

当然了，有人会说，那我上传到服务器之前将其压缩成一个zip包，然后在Linux通过unzip命令解压。这种方式我以前也这么干过。
当我后来发现将动态web项目导出war包，直接通过winscp上传到tomcat对应的目录下，在当前目录就会产生一个文件夹，该文件夹主要是web相关的资源，还有就是java产生的编译文件class等。

由此可以推出动态web项目常规部署方式有这么几种?
a.直接上传到tomcat对应的目录下;
b.先打成zip包然后再传输到tomcat对应的目录下;
c.本地导出war包，然后在传输到tomcat对应的目录下;

其实a和b是一样的，c则是利用Eclipse的导出war功能来实现的。
<!--more-->

#### 2.maven项目

maven项目的部署以war项目为例，直接通过mvn install 或者mvn clean package直接打包上传到服务器上，就即可完成部署。
当然了，还可以通过写一个脚本利用git clone的特性加上maven，也可以完成快速部署


来个小结:
现在使用动态web项目都是一些老公司维护一些老的项目，总而言之，现在大部分都在用mavne，当然，也不排除有一部分用grandle或ant等。
其实发现用maven以后除了有些时候导入依赖(依赖之间因版本冲突问题，为此我感到烦之外，其它都还好)。

说完这两种项目部署后，下面我再说springboot打包成jar，如何在Linux上持久运行。


## 二、springboot打成的jar如何在Linux上持久运行

首先呢？你本地要有一个springboot的项目，如果没有可以参考我的这篇博客写一个,[springboot入门程序](https://www.cnblogs.com/youcong/p/8098861.html)

然后呢？你要有一个虚拟机搭建一个Linux服务器或者是远程服务器(阿里云或者腾讯云、百度云、美团云等)。


再然后，你还要有一个winscp，winscp官网地址为:https://winscp.net/eng/docs/lang:chs(你可以去官网下载)

最后将springboot打包(确保本地运行没有问题)，利用winscp上传到Linux上。

通过该命令运行jar包:
```
nohup java -jar blog.jar > system.log 2>&1 &
```
会在本地有一个system.log文件产生，通过该文件你可以看到对应的日志输出。

下面我们对这条命令进行分析

nohub一般形式为如下:

nohub command &


但是当你退出账户时，仍然会停止对应的进程。

所以这就需要你在后面添加 2>&1 &(相当于正常退出，仍保持命令在后台运行)

上面这个command正好对上java -jar blog.jar > system.log

">" 输出重定向，通常用于输出日志

本文主要参考该地址:https://www.cnblogs.com/createhappy/p/9375874.html