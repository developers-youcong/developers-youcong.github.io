---
title: 'An internal error occurred during: Initializing Java Tooling.'
date: 2019-04-22 10:42:45
tags: "Eclipse"
---
详细错误信息:
```
An internal error occurred during: "Initializing Java Tooling".
java.lang.NullPointerException

```

问题原因:不合理关闭Eclipse导致的
<!--more-->
问题的影响:
比如你要启动Eclipse某个JavaEE应用时你会发现报错，总是显示某某类找不到，针对某某类找不到，要么就是那个类路径有问题，要么就是项目没有更新完全需要update project。通常update project就好。不过现在无论你update project多少次都没有用，因为不是这个原因。原因就是上述的错误信息。

解决办法:
两种解决办法，如下所示:
(1)重启Eclipse;
(2)删除 当前工作目录文件夹下的 /.metadata/.plugins/org.eclipse.core.resources/.project。就是把初始化的项目删除，然后打开eclipse以后可以重新初始化;

参考链接如下:
[An internal error occurred during: "Initializing Java Tooling". Eclipse启动发生的错误](https://blog.csdn.net/u013100581/article/details/52942641)