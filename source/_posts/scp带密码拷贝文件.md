---
title: scp带密码拷贝文件
date: 2020-09-24 22:35:28
tags: "Linux"
---

应用场景:
将B服务器的文件传输到A服务器。
<!--more-->
核心命令:
```
sshpass -p 123456 scp ubuntu@192.168.52.1:/home/ubuntu/"TEST"''$(date +"%Y")''$[$(date +"%j"+$i)] /home/test

```
需要安装sshpass。

ubuntu16.04执行:
```
sudo apt-get install sshpass

```

centos7执行:
```
yum -y install sshpass

```

为什么不纯用SCP?
主要考虑到服务器之间传输需要密码授权。
当然了，也可以免密，但是免密一般来说不太安全，同时呢？考虑到是从B服务器拉取文件到A服务器，scp做起来比较麻烦。
如果是纯粹从B服务器免密传文件到A服务器的话，scp免密做起来很方便。
关于Linux免密传输，可以参考我的这篇博客:
[Linux远程传输文件免密码](https://www.cnblogs.com/youcong/p/10809056.html)


本文主要参考资料:
[scp带密码拷贝文件](https://blog.csdn.net/d1240673769/article/details/99947375)
