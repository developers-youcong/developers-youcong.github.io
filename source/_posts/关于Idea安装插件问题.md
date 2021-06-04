---
title: 关于Idea安装插件问题
date: 2020-02-28 22:46:56
tags: "IDEA"
---

今天安装一个叫apidoc的插件出现了问题，Idea安装插件同Eclipse安装插件一样简单，都支持线上搜索直接安装和本地下载后安装。
<!--more-->

我的Idea是2018年的版本，不知道由于什么原因搜索不到插件。

然后我参考这个链接解决搜索不到插件问题:

Idea搜索不了插件解决办法:
https://blog.csdn.net/chenchunlin526/article/details/85339082

使用这个方法解决后，突然发现还是不行，插件虽然都出来了，但是搜索关键字失效，没有用。实在没有办法我只能采取本地安装插件的方式。

Idea插件市场(PC端):
https://plugins.jetbrains.com/idea

本地安装插件很简单，步骤如下:

(1)下载地址:https://plugins.jetbrains.com/idea

(2)搜索插件名称

(3)下载IDEA对应版本号的插件

(4)将插件解压到IDEA安装目录的plugs文件夹下后重启Idea

如果不太明白的话，可以参考如下链接进行本地安装:
https://jingyan.baidu.com/article/3d69c5513e5953f0cf02d7b4.html

不过有一个注意事项，就是IDEA插件与IDEA版本需要匹配，不然可能会报如下错误(英语不错的，看到该信息就明白了原因，所以作为程序员必须要把英语学好):
```
plugin xxxx is incompatible

```

该错误解决办法就是安装与Idea版本相匹配的插件。

对该错误有疑惑的话，可参考如下链接:

Intellij IDEA 安装插件 报 ‘plugin xxxx is incompatible‘ 解决方案:
https://blog.csdn.net/github_38410229/article/details/79475745