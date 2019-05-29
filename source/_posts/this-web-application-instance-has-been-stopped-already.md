---
title: this web application instance has been stopped already
date: 2019-04-17 14:56:49
tags: "Linux"
---

详细错误信息如下:
```
[mybatis-plus MapperRefresh] org.apache.catalina.loader.WebappClassLoaderBase.checkStateForResourceLoading Illegal access: this web application instance has been stopped already. Could not load [META-INF/services/javax.xml.xpath.XPathFactory]. The following stack trace is thrown for debugging purposes as well as to attempt to terminate the thread which caused the illegal access.
 java.lang.IllegalStateException: Illegal access: this web application instance has been stopped already. Could not load [META-INF/services/javax.xml.xpath.XPathFactory]. The following stack trace is thrown for debugging purposes as well as to attempt to terminate the thread which caused the illegal access.

```

关键信息如下:
```
this web application instance has been stopped already

```
<!--more-->
初次看到这个问题，我也是一头雾水，因为没有影响正常的项目对外服务，但是严重影响线上对接客户端时日志的阅读。

后来通过搜索知道了原因，原来是因为服务器上其中两个tomcat下的webapps项目相同导致的，而且这两个tomcat又是运行的，所以才导致这样的错误，于是我将另外的测试tomcat去除掉，问题就迎刃而解。

问题解决参考链接:
[关于tomcat启动报“this web application instance has been stopped already”的处理](https://www.cnblogs.com/xxyBlogs/p/5536731.html)

[org.apache.catalina.loader.WebappClassLoaderBase.checkStateForResourceLoading Illegal access](https://blog.csdn.net/zl544434558/article/details/49095591)