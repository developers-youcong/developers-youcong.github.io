---
title: Linux当前用户如何免密登录
date: 2020-12-19 14:31:22
tags: "Linux"
---
最近因某些问题研究需要用到这相关的知识，今天做个记录。
Linux当前用户如何免密登录，通常免密登录的应用场景主要是跨服务器文件传输或者跨服务器进行某些操作需要用到。

关于跨服务器文件传输可以参考早年我写的这篇文章:
[Linux远程传输文件免密码](https://www.cnblogs.com/youcong/p/10809056.html)

关于当前用户如何免密登录，很简单按照如下步骤操作即可:
<!--more-->
```
ssh-keygen -t rsa #一路回车，不做任何操作。

cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys #将生成的key写入authorized_keys文件中。

chmod 600 ~/.ssh/authorized_keys #其中600权限的意思是设置拥有者可读写，其他人不可读写执行。

ssh localhost #初次输入会弹出一个确认提示，输入yes即可，接下来执行 ssh localhost命令就不需要再次输入。

```