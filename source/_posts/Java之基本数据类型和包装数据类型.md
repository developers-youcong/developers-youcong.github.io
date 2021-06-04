---
title: Java之基本数据类型和包装数据类型
date: 2021-03-21 20:35:57
tags: "Java"
---
最近因为如下几个问题，有些疑惑，于是展开研究。
- 变量尽量不要使用包装类型，强烈建议使用基本数据类型，是出于哪些考虑？
- ORM映射的实体类为何建议使用包装数据类型，是出于哪些考虑？
<!--more-->

## 一、变量尽量不要使用包装类型，强烈建议使用基本数据类型，是出于哪些考虑？
最主要是性能方面的考虑。以int和Integer来说，两者的存储原理不一样，int属于基本数据类型，不存在引用概念，其数据存储在栈上；而Integer，属于继承自Object类，按照Java存储对象的内存模型来存储，引用存储在栈上，对象数据存储在堆中。

### 1.堆和栈哪个更快？
- (1)访问时间上，栈只需访问一次，而堆需要访问两次，一次访问取得指针，二次访问才能真正获取数据；
- (2)分配和释放，堆在分配和释放时都要调用函数(MALLOC,FREE)，比如分配时会到堆空间去寻找足够大小的空间(因为多次分配释放后会造成空洞)，这些都会花费一定的时间，而栈则不需要。

综上上面两点，int的性能比Integer更好，而int同short、int、long、double、floag、boolean、char、byte等属于基本数据类型，而基本数据对象存储在栈中，自然基本数据类型从性能上比包装类型更强。

参考资料如下:
[Java为什么需要保留基本数据类型](https://blog.csdn.net/findmyself_for_world/article/details/50094991)

[int和Integer的区别，变量尽量不要定义为包装类，尽量使用基本类型。](https://blog.csdn.net/yuxiaoshuangshuang/article/details/80677882)

[实体类中用基本类型好,还是用包装类型好?](https://blog.csdn.net/ethan_10/article/details/79812112)

## 二、ORM映射的实体类为何建议使用包装数据类型，是出于哪些考虑？
包装类型可以赋值为null，表示空，但基本类型不能赋值为null。
而实际中数据表里面的每一行数据并不能保证都是不等于null的，使用包装类比较保险，避免报错影响数据服务的对外提供。

### 1.Java中为何要有包装类，包装类的目的是什么？
从我使用的角度来看，因为有包装类能减少数据类型转换的麻烦，同时List、Set、Map中像基本数据类型(int、short、long等)这样的数据是放不进去的，只能转为包装类型才能放进去，而List、Set、Map在开发中又最为普遍常用。

参考资料如下:
[Java为什么要有包装类](https://www.cnblogs.com/vegetableDD/p/11763009.html)
[Java 中为什么设计了包装类](https://cloud.tencent.com/developer/article/1803380)
[Java为什么要对基本类型进行包装（装箱和拆箱）](https://www.jianshu.com/p/bb4ff1c9853b)