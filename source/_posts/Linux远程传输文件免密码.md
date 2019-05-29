---
title: Linux远程传输文件免密码
date: 2019-04-29 13:57:15
tags: "Linux"
---

首先为什么Linux远程传输要免密码?
手动使用scp命令传输每次都要输密码太过麻烦了。

开发中有一句话，能复制粘贴尽量不要手打。

运维中有一句话，能脚本化实现尽量不要手动执行。


**远程传输文件免密码的目的:**
在于为了保证公司数据安全，将相关的备份文件传输到一个或多个备份服务器上防止服务器上面的数据因运维人员失误或者相关运营商失误而导致的严重后果。

A服务器地址：192.168.1.126，下面简称A 
B服务器地址：192.168.1.128，下面简称B

步骤如下:
<!--more-->
## 在A中生成密钥对
```
ssh-keygen -t rsa -P ""

```
执行上述命令，一路回车，会在当前登录用户的home目录下的.ssh目录下生成id_rsa和id_rsa.pub两个文件，分别代表密钥对的私钥和公钥。

## 拷贝A的公钥(id_rsa.pub)
将其拷贝到B的root用户home目录为例:
```
scp /root/.ssh/id_rsa.pub root@192.168.1.128:/root

```


## 登录B
拷贝A的id_rsa_pub内容到.ssh目录下的authorized_keys文件中
```
cd /root
cat id_rsa.pub >> .ssh/authorized_keys

```


## 此时在A中用ssh登录B或想B传输文件将不需要密码
```
ssh root@192.168.1.128或
scp test.txt root@192.168.1.128：/home/test/

```

参考资料如下:
[Linux远程传输文件免密码验证登陆和拷贝文件](https://blog.csdn.net/zhan570556752/article/details/80547063)