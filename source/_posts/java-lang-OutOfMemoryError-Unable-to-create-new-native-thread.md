---
title: 'java.lang.OutOfMemoryError: Unable to create new native thread'
date: 2020-11-05 19:39:36
tags: "Java"
---

错误信息:
```
java.lang.OutOfMemoryError: Unable to create new native thread 

```

从字面意思我们就很好理解，这是因为内存不足导致的错误，内存不足不能创建新的线程。

于是我搜索了一下，找到了解决方案:
<!--more-->
### 1.排查应用是否创建了过多的线程
通过jstack命令排查。

### 2.调整操作系统线程数阈值
操作系统会限制进程允许创建的线程数，使用ulimit -u命令查看限制。某些服务器上此阈值设置的过小，比如1024。一旦应用创建超过1024个线程，就会遇到java.lang.OutOfMemoryError: unable to create new native thread问题。如果是这种情况，可以调大操作系统线程数阈值。

### 3.增加机器内存
如果上述两项未能排除问题，可能是正常增长的业务确实需要更多内存来创建更多线程。如果是这种情况，增加机器内存。

### 4.减小堆内存
线程不在堆内存上创建，线程在堆内存之外的内存上创建。所以如果分配了堆内存之后只剩下很少的可用内存，依然可能遇到java.lang.OutOfMemoryError: unable to create new native thread。考虑如下场景：系统总内存6G，堆内存分配了5G，永久代512M。在这种情况下，JVM占用了5.5G内存，系统进程、其他用户进程和线程将共用剩下的0.5G内存，很有可能没有足够的可用内存创建新的线程。如果是这种情况，考虑减小堆内存。

### 5.减少进程数
这和减小堆内存原理相似。考虑如下场景：系统总内存32G，java进程数5个，每个进程的堆内存6G。在这种情况下，java进程总共占用30G内存，仅剩下2G内存用于系统进程、其他用户进程和线程，很有可能没有足够的可用内存创建新的线程。如果是这种情况，考虑减少每台机器上的进程数。

### 6.减少线程栈大小
线程会占用内存，如果每个线程都占用更多内存，整体上将消耗更多的内存。每个线程默认占用内存大小取决于JVM实现。可以利用-Xss参数限制线程内存大小，降低总内存消耗。例如，JVM默认每个线程占用1M内存，应用有500个线程，那么将消耗500M内存空间。如果实际上256K内存足够线程正常运行，配置-Xss256k，那么500个线程将只需要消耗125M内存。（注意，如果-Xss设置的过低，将会产生java.lang.StackOverflowError错误）。

最后我是用2办法解决的。
这与我之前遇到过的这样的错误一样，错误信息如下:
```
Cannot create GC thread. Out of system resources.

```

参考文章解决方案:[Cannot create GC thread. Out of system resources.](https://www.cnblogs.com/youcong/p/13865984.html)


本文的六个解决方案主要参考了这篇文章:
[解决OutOfMemoryError: unable to create new native thread问题](https://blog.csdn.net/wchgogo/article/details/78185643)
