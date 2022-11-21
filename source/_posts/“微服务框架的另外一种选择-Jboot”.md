---
title: '微服务框架的另外一种选择:Jboot'
date: 2022-10-16 13:25:54
tags: "开源项目"
---


## 一、什么是Jboot?
Jboot 是一个基于 JFinal、JFinal-Undertow、Dubbo、Seata、Sentinel、ShardingSphere、Nacos 等开发的微服务框架， 帮助开发者降低微服务开发门槛。同时完美支持在 idea、eclipse 下多 maven 模块，对 java 代码、html、css、js 等资源文件进行热加载。爽爽开发，快乐生活。
<!--more-->

## 二、Jboot具有哪些特征？
- 1、基于 JFinal 的 MVC + ORM 快速开发。
- 2、基于 ShardingSphere + Seata 分布式事务 和 分库分表。
- 3、基于 Dubbo 或 Motan 的 RPC 实现。
- 4、基于 Sentinel 的分布式限流和降级。
- 5、基于 Apollo 和 Nacos 的分布式配置中心。
- 6、基于 EhCache 和 Redis 的分布式二级缓存。


## 三、在了解与使用Jboot之前，你需要具备怎样的能力？
- 1.熟悉 Java 编程语言。
- 2.熟悉 maven 的基本原理。
- 3.熟悉 IntelliJ IDEA 或者 Eclipse 等编辑器的使用。


## 四、关于Jboot的资料有哪些？
官方网站:
http://www.jboot.com.cn/

官方文档:
http://www.jboot.com.cn/docs/

Gitee源代码:
https://gitee.com/JbootProjects/jboot

Github源代码:
https://github.com/yangfuhai/jboot

## 五、Jboot相关的生态有哪些？
其中最著名的生态便是JPress！！！

JPress官方网站:
http://www.jpress.cn/

JPress商业案例:
http://www.jpress.cn/article/category/case

JPress模板:
http://www.jpress.cn/product/category/template

JPress用户手册:
http://doc.jpress.cn/manual/

JPress开发文档:
http://doc.jpress.cn/development/

Gitee源代码:
https://gitee.com/JPressProjects/jpress

Github源代码:
https://github.com/JPressProjects/jpress

## 六、如何使用Jboot？

### 1.引入Maven依赖
```
<dependency>
    <groupId>io.jboot</groupId>
    <artifactId>jboot</artifactId>
    <version>3.15.7</version>
</dependency>

```

### 2.编写启动类
```
import io.jboot.app.JbootApplication;
import io.jboot.web.controller.JbootController;
import io.jboot.web.controller.annotation.RequestMapping;

@RequestMapping("/")
public class IndexController extends JbootController {

    public void index() {
        renderText("Hello World Jboot");
    }


    public static void main(String[] args) {
        JbootApplication.run(args);
    }
}

```

接下来如果想集成更多的功能，可以参考对应的官方文档即可！！！
