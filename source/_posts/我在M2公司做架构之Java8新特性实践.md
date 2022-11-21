---
title: 我在M2公司做架构之Java8新特性实践
date: 2021-11-12 20:00:42
tags: ["架构","职业生涯","技术思考与实践"]
---

通过[JDK官网下载](https://www.oracle.com/java/technologies/downloads/)了解，JDK已经出到17了。

从我个人的使用经验来看，从JDK6到JDK8，做的系统也随之不一样，传统的老项目偏JDK5、JDK6、JDK7等，新的互联网项目或者是大数据分析相关的项目一般用JDK8。
<!--more-->

# 一、JDK8新特性有哪些？
- Lambda表达式；
- 新的日期API；
- 引入Optional；
- 新增Base64加解密API；
- 接口的默认方法和静态方法；
- 新增方法引用格式；
- 新增Stream类；
- 注解相关的改变；
- 支持并行数组；
- 对并发类的扩展。

# 二、我在实际开发中用到哪些JDK8的新特性？
实际开发中我在基于数据的计算与组装处理等用到下面，这些是我比较常用的，针对Map、List做相关的操作。

```
//分组
Map<String, List<T>> yearData = allData.stream().collect(Collectors.groupingBy(T::getYear));

//过滤筛选(单条件)
List<T> filterList = appleList.stream().filter(a -> a.getName().equals("YC")).collect(Collectors.toList());

//过滤筛选(多条件)
List<T> filterList = dayVoList.
                      stream().filter(a -> a.getYEAR().equals(item)).collect(Collectors.toList())
                      .stream().filter(a -> a.getPrice() != "0" && a.getPrice() != "0.0").collect(Collectors.toList());
					  
//合并List去重
List<String> result = Stream.of(Lists.newArrayList("A", "B", "C"), Lists.newArrayList("A", "B"))
  .flatMap(Collection::stream).distinct().collect(Collectors.toList());
  
//合并List不去重
List<String> result = Stream.of(Lists.newArrayList("A", "B", "C"), Lists.newArrayList("A", "B"))
  .flatMap(Collection::stream).collect(Collectors.toList());
  
//倒序
List<T> api_list = apiData
               .stream().sorted(Comparator.comparing(T::getID).reversed()).collect(Collectors.toList());
			   
//正序
List<T> api_list = apiData
               .stream().sorted(Comparator.comparing(T::getID).collect(Collectors.toList());
			   
//List去重
List<T> primaryFilterData = apiData.stream().collect(
               Collectors.collectingAndThen(
                       Collectors.toCollection(() -> new TreeSet<>(Comparator.comparing(T::getName))), ArrayList::new));
					   
//分页+条件筛选
List<T> handeList = reportVoList
                    .stream().filter(s -> s.getCode() < code_conditon).collect(Collectors.toList())
                  .stream().skip((reqDTO.getCurPage() - 1) * reqDTO.getPageSize()).limit(reqDTO.getPageSize()).
                            collect(Collectors.toList());
                            
```

关于Java8相关特性的详细介绍和使用，部分因为我没有用过，就不说了，如下的系统文档和资料以供大家参考：
[Java8新特性](https://www.bookstack.cn/read/android_interview/java-basis-java-8.md)


