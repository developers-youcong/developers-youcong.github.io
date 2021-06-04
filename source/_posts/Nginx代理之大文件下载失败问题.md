---
title: Nginx代理之大文件下载失败问题
date: 2021-02-28 14:15:36
tags: "Linux"
---

错误详细信息:
```
Cloning into 'aplanmis-project'...
remote: Enumerating objects: 176887, done.
remote: Counting objects: 100% (176887/176887), done.
remote: Compressing objects: 100% (75181/75181), done.
error: RPC failed; curl 18 transfer closed with outstanding read data remaining
fatal: the remote end hung up unexpectedly
fatal: early EOF
fatal: index-pack failed

```
<!--more-->

上面的错误信息导致我无法git clone项目到本地。

通过关键字搜索找到对应的解决方案如下:

### 1.缓冲区设置大小
```
git config --global http.postBuffer 524288000 　　    # 2GB
git config --global http.postBuffer 2097152000　　    # 2GB
git config --global http.postBuffer 3194304000 　　   # 3GB
```

### 2.网络原因
```
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999

```

### 3.少git clone一些
```
git clone git地址 --depth 1

```

上面的三个办法我均尝试，但仍然没有解决这个问题。于是我仔细一想，自建的git通过nginx代理，可能与nginx有关，然后我关键字搜索nginx大文件下载失败问题，于是找到了解决方案:

参考了该链接，如下:
[Nginx反向代理导致大文件下载失败](http://blog.chinaunix.net/uid-20332519-id-5755724.html)

该链接提供了两个解决方案，我尝试了第一个解决方案就解决了该问题。
在nginx代理配置如下(location标签配置即可):
```
proxy_redirect default;
proxy_buffering off;

```