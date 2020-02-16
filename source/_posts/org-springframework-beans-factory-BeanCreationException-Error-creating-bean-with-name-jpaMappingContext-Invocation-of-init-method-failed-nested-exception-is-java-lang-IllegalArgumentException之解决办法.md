---
title: >-
  org.springframework.beans.factory.BeanCreationException: Error creating bean
  with name 'jpaMappingContext': Invocation of init method failed; nested
  exception is java.lang.IllegalArgumentException之解决办法
date: 2019-11-30 19:32:57
tags: "SpringBoot"
---

错误产生背景:
将之前用Eclipse写的Blog项目迁移到Idea上面。Ecilpse项目一直是没有问题的。
<!--more-->
错误原因分析:
原因是项目依赖中引入的jpa,另外也与Idea比较智能也有关系

解决办法:

第一，在启动类中加上如下注解代码
```
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class,JpaRepositoriesAutoConfiguration.class,HibernateJpaAutoConfiguration.class})

```


第二，maven依赖中排除该依赖(尽管在pom.xml去除了，但是不要忘记maven有依赖传递的特性)


参考解决办法链接(stackoverflow上面找到的解决方案):
https://stackoverflow.com/questions/40738818/illegalargumentexception-at-least-one-jpa-metamodel-must-be-present