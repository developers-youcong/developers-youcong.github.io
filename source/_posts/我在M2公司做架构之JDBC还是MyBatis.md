---
title: 我在M2公司做架构之JDBC还是MyBatis
date: 2022-04-05 10:35:10
tags: ["架构","职业生涯","技术思考与实践"]
---

本文主要内容如下:

- 1.JDBC与MyBatis的区别有哪些？
- 2.关于JDBC、MyBatis等我过去写了哪些文章？
- 3.在M2公司做架构的时候，JDBC与MyBatis我又是如何用的？

<!--more-->

## 一、JDBC与MyBatis的区别有哪些？
- 1.从层次上看，JDBC是较底层的持久层操作方式，而MyBatis都是在JDBC的基础上进行了封装使其更加方便程序员对持久层的操作。
- 2.从功能上看，JDBC就是简单的建立数据库连接，然后创建statement，将sql语句传给statement去执行，如果是有返回结果的查询语句，会将查询结果放到ResultSet对象中，通过对ResultSet对象的遍历操作来获取数据；MyBatis是将sql语句中的输入参数和输出参数映射为java对象，sql修改和优化比较方便。
- 3.从使用上看，如果进行底层编程，而且对性能要求极高的话，应该采用JDBC的方式；如果要灵活使用sql语句的话建议采用MyBatis框架。

## 二、关于JDBC、MyBatis等我过去写了哪些文章？
[单例模式和JDBC](https://www.cnblogs.com/youcong/p/7841994.html)
[MyBatis+Hibernate+JDBC对比分析](https://www.cnblogs.com/youcong/p/8719778.html)
[我对SSM框架的思考(包含Spring、SpringMVC、MyBatis系列文章)](https://youcongtech.com/2021/11/07/%E6%88%91%E5%AF%B9SSM%E6%A1%86%E6%9E%B6%E7%9A%84%E6%80%9D%E8%80%83/)

## 三、在M2公司做架构的时候，JDBC与MyBatis我又是如何用的？
- 1.基于数据可视化展示，使用MyBatis；
- 2.基于后台管理(CURD之类的)，使用MyBatis；
- 3.基于数据采集、处理、存储等过程，视实际情况而定，如果数据量非常大，将采用JDBC执行批量处理(前面提到过，性能要求极高，所以使用底层这种方式)。