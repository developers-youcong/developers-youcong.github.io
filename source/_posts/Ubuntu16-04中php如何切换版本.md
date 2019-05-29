---
title: Ubuntu16.04中php如何切换版本
date: 2019-04-10 14:09:51
tags: "Linux"
---

其实就是一条Linux命令,如下:
```
sudo update-alternatives --config php
```
<!--more-->
会出现下面选项:
```
There are 2 choices for the alternative php (providing /usr/bin/php).

  Selection    Path             Priority   Status
------------------------------------------------------------
* 0            /usr/bin/php7.1   71        auto mode
  1            /usr/bin/php7.0   70        manual mode
  2            /usr/bin/php7.1   71        manual mode
Press <enter> to keep the current choice[*], or type selection number:
```
输入其中一项数字即可实现php版本切换

如何判断版本是否切换成功？
`php -v`命令进行前后版本对比即可看出。

