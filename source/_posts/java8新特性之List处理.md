---
title: java8新特性之List处理
date: 2020-09-05 16:07:38
tags: "Java"
---

分组:
```
Map<String, List<T>> yearData = allData.stream().collect(Collectors.groupingBy(T::getYear));


```
<!--more-->

过滤筛选(单条件):
```
List<T> filterList = appleList.stream().filter(a -> a.getName().equals("YC")).collect(Collectors.toList());


```

过滤筛选(多条件):
```
  List<T> filterList = dayVoList.
                        stream().filter(a -> a.getYEAR().equals(item)).collect(Collectors.toList())
                        .stream().filter(a -> a.getPrice() != "0" && a.getPrice() != "0.0").collect(Collectors.toList());


```