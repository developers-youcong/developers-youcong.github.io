---
title: Cannot create GC thread. Out of system resources.
date: 2020-10-20 21:44:05
tags: "Linux"
---
#### 错误信息:
```
Cannot create GC thread. Out of system resources.

```
<!--more-->
#### 问题背景:
使用普通用户部署项目报这样的错误信息。

#### 错误原因:
Linux是有文件句柄限制的，而且Linux默认不是很高，一般都是1024，生产服务器用其实很容易就达到这个数量。
也就是说普通用户有软硬件的限制。这不是授权就可以解除的。


#### 解决办法:
```
cd /etc/security/limits.d

vim 20-nproc.conf 

```

修改如下内容:

文件原内容:
```
*          soft    nproc     4096
root       soft    nproc     unlimited

```

文件修改后的内容:
```
*          soft    nproc     unlimited
root       soft    nproc     unlimited

```

最终使用普通用户部署就不会再出现这样的错误了。

另外之所以做这样的限制是为了避免前端请求，后端处理时，一些恶意的请求导致对应的进程会增加，从而占用内存。如果不做一定的限制，可能会使得整个服务器宕机。这也就是为什么前端接口要鉴权，后台要有一个机制来识别哪些是恶意的请求。之前一直觉得微服务组件Sentinel没用，但后来发现其实它还是有用的，至少它可以在一定程度上起到一个识别是否是正常请求的作用，对于哪些频繁的恶意请求，Sentinel直接转向其它提示信息，并不会走到后端真正的处理逻辑从而避免不必要的资源消耗。