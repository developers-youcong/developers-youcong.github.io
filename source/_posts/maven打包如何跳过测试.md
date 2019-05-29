---
title: maven打包如何跳过测试
date: 2019-04-16 11:44:11
tags: "Maven"
---

Maven打包如何跳过测试?
正常来说，不应该这样做，因为测试可以避免很多麻烦排除一些不必要的错误，前提是测试足够规范，这里主要指junit测试，如果junit测试有问题的话，将会直接影响到mvn install打包。
<!--more-->

**如何跳过测试，有两种办法**:

(1)使用maven命令
```
mvn test -Dmaven.test.failure.ignore=true  
mvn install -Dmaven.test.skip=true  

```

(2)在pom文件中添加如下内容
```
	<build>

		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-plugin</artifactId>
				<configuration>
					<skip>true</skip>
				</configuration>
			</plugin>
		</plugins>
	</build>

```

