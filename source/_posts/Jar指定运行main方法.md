---
title: Jar指定运行main方法
date: 2020-11-06 21:50:52
tags: "Java"
---
最近在试验某个功能遇到这样的需求，并不需要项目一直运行，这是在某个特定的时候运行即可，而且只运行main方法里面的应用程序。
<!--more-->
这里我没有用grandle，用的是Maven，主要在pom.xml配置如下内容即可:
```
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId>
        <version>2.3</version>
        <configuration>
          <appendAssemblyId>false</appendAssemblyId>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
          <archive>
            <manifest>
              <addClasspath>true</addClasspath>
              <classpathPrefix>lib/</classpathPrefix>
              <mainClass>com.blog.test.MainTest</mainClass>
            </manifest>
          </archive>
        </configuration>
        <executions>
          <execution>
            <id>make-assembly</id>
            <phase>package</phase>
            <goals>
              <goal>assembly</goal>
            </goals>
 
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>

```

本文参考资料如下:
[执行jar包中指定main方法](https://blog.csdn.net/xiaoguangtouqiang/article/details/82182654)