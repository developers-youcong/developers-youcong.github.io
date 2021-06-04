---
title: SpringBoot使用Maven打包时减小jar包的大小方法
date: 2020-08-23 11:20:32
tags: "Maven"
---

我在没使用maven插件压缩打包的时候，一个应用打包基本上100M以上，以我个人博客中的一个管理微服务模块来说，打包成功后生成的jar就123M左右。为此我搜索了下，研究如何减少jar包体积大小的方法，不料真还找到了。
<!--more-->
步骤总结如下:

### 第一步添加插件
maven对应的微服务模块中pom.xml增加如下内容:
```
  <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.springcloud.blog.admin.BlogAdminApplication</mainClass>
                    <layout>ZIP</layout>
                    <includes>
                        <include>
                            <!-- 排除所有Jar -->
                          <groupId>nothing</groupId>
                            <artifactId>nothing</artifactId>
                        </include>
                    </includes>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>

    </build>

```

#### 第二步执行打包命令
```
mvn clean install -Dmaven.test.skip=true

```

#### 第三步运行jar包
```
java -Dloader.path="lib/" -jar blog-admin-1.0-SNAPSHOT.jar

```
blog-admin-1.0-SNAPSHOT.jar是我自己的应用，改成你们对应的即可。

注意事项:
在此以前必须要把lib抽取出来，lib这个文件夹主要放jar包的(微服务框架所涉及的jar文件)。

那么如何打出这个lib来的，只需去除第一步的插件即可(也就是常规打包方式)，常规打包抽取lib后，再通过减少jar包体积的步骤来进行打包。

通常打出的jar，以我blog-admin这个应用为例，原本打出来的是123M(没有使用插件)，使用插件后打包是不到2M。

本文参考资料如下:
[SpringBoot使用Maven打包时减小jar包的大小方法](https://blog.csdn.net/qq_42428264/article/details/107079961)