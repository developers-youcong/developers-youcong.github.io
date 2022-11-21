---
title: xjar加密jar文件
date: 2021-07-22 19:42:13
tags: "Java"
---

## 一、为什么要加密jar文件？
解压jar文件可获取该项目的配置文件和源代码，配置文件中通常存有数据库连接、服务应用配置等信息。通常黑客攻入服务器后(获取某个普通用户的账号)，为什么能轻易的绑架数据库，在于通过普通用户解压jar或war文件，从而得到数据库账号实现备份和删库(备份就是为了敲诈勒索)。那么如何防范呢？首先从服务器层面防范，可以阅读这篇文章:[服务器安全策略之思考与实践](https://youcongtech.com/2021/07/16/%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%89%E5%85%A8%E7%AD%96%E7%95%A5%E4%B9%8B%E6%80%9D%E8%80%83%E4%B8%8E%E5%AE%9E%E8%B7%B5/)

<!--more-->

加密jar文件就是为了防止黑客解压后获取一些隐秘的信息，从而造成数据库数据泄漏或其它危害影响。

## 二、在Java中加密jar有哪些方案？
方案有很多，但此次我要讲的方案之一就是使用xjar框架进行加密。
Xjar对应的开源代码地址为(有详细的介绍，故不再赘述):
https://github.com/core-lib/xjar

## 三、Xjar方案具体实施

### 1.新建项目，添加Maven依赖
```
    <!-- 设置 jitpack.io 仓库 -->
    <repositories>
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>
    <!-- 添加 XJar 依赖 -->
    <dependencies>
        <dependency>
            <groupId>com.github.core-lib</groupId>
            <artifactId>xjar</artifactId>
            <version>4.0.2</version>
            <!-- <scope>test</scope> -->
        </dependency>
    </dependencies>

```

### 2.执行核心方法即可
```
public class Test {
    public static void main(String[] args) throws Exception {
        XCryptos.encryption()
                .from("D:\\test\\jar\\crawler-1.0-SNAPSHOT.jar")
                .use("io.xjar")
                .include("/io/xjar/**/*.class")
                .include("/mapper/**/*Mapper.xml")
                .exclude("/static/**/*")
                .exclude("/conf/*")
                .to("D:\\test\\jar\\test.jar");
    }
}

```