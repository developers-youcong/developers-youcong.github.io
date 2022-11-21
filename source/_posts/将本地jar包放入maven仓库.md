---
title: 将本地jar包放入maven仓库
date: 2021-07-15 21:36:27
tags: "Maven"
---

因为业务需要，需要引入非Maven的jar，项目中使用Maven来管理依赖包，如今非Maven依赖的jar该如何引入呢？很简单，执行如下命令，将jar变为本地Maven仓库的依赖即可：
<!--more-->
```
mvn install:install-file -Dfile=blog-mail-1.0.0.jar -DgroupId=com.blog -DartifactId=mail -Dversion=1.0.0 -Dpackaging=jar

```
**参数详解:**
Dfile：相当于普通的jar包文件。
Dgroup：项目组织标识符，相当于Java包结构。
DartfactId：项目唯一标识符，相当于项目名称。
Dversion：版本号。
Dpackaging：打包类型(war或者jar等)。

打包完毕后，去对应的Maven仓库目录找到pom.xml坐标，然后引入到项目中的pom.xml即可，如下所示:
```
<dependency>
  <groupId>com.blog</groupId>
  <artifactId>blog.mail</artifactId>
  <version>1.0.0</version>
</dependency>

```

参考资料如下:
[将本地jar包放入maven仓库](https://blog.csdn.net/tobeng/article/details/80703753)