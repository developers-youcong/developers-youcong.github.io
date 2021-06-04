---
title: 'IDEA java.lang.OutOfMemoryError: Java heap space'
date: 2021-04-12 21:21:53
tags: "Idea"
---

### 一、错误背景
本地开发环境，使用PostMan或Swagger请求A微服务，而A微服务需要将数据传递给B微服务,A微服务的控制台开始报错，使得A微服务没有得到正确的响应。
<!--more-->

### 二、关键错误信息
```
IDEA java.lang.OutOfMemoryError: Java heap space
```

### 三、错误原因
错误原因是因为A微服务所暴露的接口，接收的数据量实在是太大了，导致Idea内存溢出。

### 四、解决办法
在Idea中点击Edit Configurations，将VM options设置大一点，就能解决这个问题。我将其设置为-Xmx5048m才解决这个问题。

解决问题办法，主要参考如下链接:
[IDEA java.lang.OutOfMemoryError: Java heap space](https://blog.csdn.net/weixin_44795273/article/details/107799686)

### 六、OutOfMemoryError的多种情况、原因以及解决方案(本文仅针对这五种进行较为详细的说明)

#### 1.java.lang.OutOfMemoryError: Java heap space
**原因:**Heap内存溢出，意味着Young和Old generation的内存不够。
**解决方案:**调整java启动参数 -Xms -Xmx 来增加Heap内存。

#### 2.java.lang.OutOfMemoryError: unable to create new native thread
**原因:**Stack空间不足以创建额外的线程，要么是创建的线程过多，要么是Stack空间确实小了。
**解决方案:**由于JVM没有提供参数设置总的stack空间大小，但可以设置单个线程栈的大小；而系统的用户空间一共是3G，除了Text/Data/BSS/MemoryMapping几个段之外，Heap和Stack空间的总量有限，是此消彼长的。因此遇到这个错误，可以通过两个途径解决：1.通过-Xss启动参数减少单个线程栈大小，这样便能开更多线程（当然不能太小，太小会出现StackOverflowError）；2.通过-Xms -Xmx 两参数减少Heap大小，将内存让给Stack（前提是保证Heap空间够用）。

##### 3.java.lang.OutOfMemoryError: PermGen space
**原因:**Permanent Generation空间不足，不能加载额外的类。
**解决方案:**调整-XX:PermSize= -XX:MaxPermSize= 两个参数来增大PermGen内存。一般情况下，这两个参数不要手动设置，只要设置-Xmx足够大即可，JVM会自行选择合适的PermGen大小。

##### 4.java.lang.OutOfMemoryError: Requested array size exceeds VM limit
**原因:**这个错误比较少见（试着new一个长度1亿的数组看看），同样是由于Heap空间不足。如果需要new一个如此之大的数组，程序逻辑多半是不合理的。
**解决方案:**修改程序逻辑吧。或者也可以通过-Xmx来增大堆内存。

##### 5.java.lang.StackOverflowError
**原因:**这也内存溢出错误的一种，即线程栈的溢出，要么是方法调用层次过多（比如存在无限递归调用），要么是线程栈太小。
**解决方案:**优化程序设计，减少方法调用层次；调整-Xss参数增加线程栈大小。

参考资料如下:
[Java中的OutOfMemoryError的各种情况及解决和JVM内存结构](https://www.cnblogs.com/duanxz/p/4901437.html)
[面试官：小伙子，你给我说一下Java中什么情况会导致内存泄漏呢？](https://cloud.tencent.com/developer/article/1690325)
[Java 内存溢出的原因和解决方法](https://www.jb51.net/article/201966.htm)