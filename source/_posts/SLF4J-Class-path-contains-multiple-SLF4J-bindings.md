---
title: 'SLF4J: Class path contains multiple SLF4J bindings.'
date: 2020-06-10 22:02:31
tags: "Maven"
---

错误信息:
```
SLF4J Warning: Class Path Contains Multiple SLF4J Bindings

```
<!--more-->
错误原因:
我个人博客系统一个爬虫组件用到webmagic，而webmagic与lomback中的slf有冲突。

解决办法(webmagic排除相关依赖即可):
```
  <!-- webmagic-->
        <dependency>
            <groupId>us.codecraft</groupId>
            <artifactId>webmagic-core</artifactId>
            <version>${webmagic.version}</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

```

参考解决问题地址:
[SLF4J Warning: Class Path Contains Multiple SLF4J Bindings](https://www.baeldung.com/slf4j-classpath-multiple-bindings)
