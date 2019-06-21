---
title: >-
  Git bash Error: Could not fork child process: There are no available terminals
  (-1)
date: 2019-06-16 19:15:24
tags: "Git"
---

错误信息:
Error: Could not fork child process: There are no available terminals (-1)
<!--more-->
截图如下:
![](Git-bash-Error-Could-not-fork-child-process-There-are-no-available-terminals-1/01.png)

解决办法:

(1)使用cmd命令tasklist，找到git bash的进程

(2)找到红色标记处
![](Git-bash-Error-Could-not-fork-child-process-There-are-no-available-terminals-1/02.png)

(3)执行命令(`taskkill /pid 9872 -t -f`)将其杀死即可

![](Git-bash-Error-Could-not-fork-child-process-There-are-no-available-terminals-1/03.png)


参考问题解决链接:
[Git bash Error: Could not fork child process: There are no available terminals (-1)](https://blog.csdn.net/qq_2300688967/article/details/78642300)
