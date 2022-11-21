---
title: Java开发者不可不知的常用工具包
date: 2022-11-19 19:38:51
tags: "Java"
---

这里我将常用工具包分为两类，一类是基于JDK本身的工具包，另外一类是基于第三方工具包。

- 1.基于JDK本身的工具包。
- 2.基于第三方工具包。
- 3.总结。

<!--more-->

### 一、基于JDK本身的工具包
- 1.java.lang
- 2.java.util
- 3.java.io
- 4.java.net
- 5.java.sql
- 6.java.math
- 7.java.awt
- 8.javax.swing
- 9.java.text
- 10.java.beans
- 11.Java.nio


### 1.java.lang
提供对Java编程语言设计至关重要的类。该包提供了程序设计的基础类，它是默认导入的包。 最重要的类是Object ，这是类层次的根。该包里面的Runnable接口、Iterable接口、Comparable接口、基本数据类型的包装类、Math、String、StringBuffer、System、Thread以及Throwable类等应用广泛。

### 2.java.util
包含集合框架，旧集合类，事件模型，日期和时间设施，国际化和其他实用程序类。该包里面的Collection接口、Iterator接口、List接口、Map接口、Queue接口、Set接口、ArrayList、Calendar、Date、HashMap、Timer类等应用广泛。

### 3.java.io
通过数据流，序列化和文件系统提供系统输入和输出。该包里面的DataInput接口、DataOutput接口、ObjectInput接口、ObjectOutput接口、InputStream、OutputStream、DataInputStream、DataOutputStream、FileInputStream和FileOutputStream等类应用广泛。

### 4.java.net
提供实现网络应用程序的类。该包里面的HttpURLConnection、InetAddress、URL、URLConnection等类应用广泛。


### 5.java.sql
提供使用Java TM编程语言访问和处理存储在数据源（通常是关系数据库）中的数据的API。该包里面的Java编程语言中映射的SQL类型接口、DriverManager等类应用广泛。

### 6.java.math
提供执行任意精度整数运算（ BigInteger ）和任意精度十进制运算（ BigDecimal ）的类。该包里面的BigDecimal和BigInteger等类应用广泛。

### 7.java.awt
包含用于创建用户界面和绘制图形和图像的所有类。

### 8.javax.swing
提供一套“轻量级”（全Java语言）组件，尽可能地在所有平台上工作。是一个为Java设计的GUI工具包。包括了图形用户界面（GUI）器件如：文本框，按钮，分隔窗格和表。提供许多比AWT更好的屏幕显示元素。

### 9.java.text
提供用于以独立于自然语言的方式处理文本，日期，数字和消息的类和接口。

### 10.java.beans
包含与开发 bean相关的类 - 基于JavaBeans架构的组件。

### 11.Java.nio
定义缓冲区，它们是数据容器，并提供其他NIO包的概述。从JDK1.4开始，Java提供了一系列改进的新IO，称为NIO（New IO）。NIO可以进行通道映射，将内核空间中的文件数据映射到用户空间，通过内存镜像直接读写内核空间的数据，不必复制文件数据，节省了时间，速度更快、效率更高，但用户程序直接操作内核空间中的文件数据，增加了内核被破坏的风险。


### 二、基于第三方工具包
- 1.Apache Commons
- 2.日志(Log4j、SLF4j和LogBack)
- 3.JSON解析库(Gson、Jackson、fastJSON)
- 4.单元测试库(Junit、Mockito、PowerMock、TestNG)
- 5.Http库(Apache HttpClient、HttpCore、OkHttp、forest)
- 6.XML解析库(Xerces, JAXB, JAXP, Dom4j, Xstream)
- 7.Excel读写库(Apache Poi、EasyExcel、EasyPoi)
- 8.数据库连接池库(Commons Pool、DBCP、Druid)
- 9.PDF处理库(iText、Apache FOP、Open Pdf、Pdf Box)
- 10.邮件API(javax.mail)
- 11.数据爬虫(JSOUP)
- 12.通用工具类库(hutool、guava)
- 13.网络库(Netty)

除以上列举还有很多很多，就不一一列举了。


## 三、总结
本文提到基于基于JDK本身的工具包与基于第三方工具包在我的[开源项目YC-Framework](https://mp.weixin.qq.com/s?__biz=MzUxODk0ODQ3Ng==&mid=2247485813&idx=1&sn=daa6e8447409672c28f07dbe094c4f28&chksm=f9805a66cef7d37061b15154a900c840da4bfdedd77cd0ea5865aee740bd1f2c01bb5cc37127&token=1774829840&lang=zh_CN&scene=21#wechat_redirect)中均可体现。

GitHub源代码地址:
https://github.com/developers-youcong/yc-framework

Gitee源代码地址:
https://gitee.com/developers-youcong/yc-framework

开源不易，如果对你有帮助，不妨给予star，鼓励一下！！！

另外，理解并应用好Java自身的工具包以及其它第三方工具包，有利于实际研发中效率的提高。
