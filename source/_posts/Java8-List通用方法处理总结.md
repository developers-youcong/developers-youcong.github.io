---
title: Java8 List通用方法处理总结
date: 2020-11-21 11:26:04
tags: "Java"
---

总结项目里使用Java8新特性对List的数据处理(用的比较多的)。
<!--more-->
### 一、分组
```
Map<String, List<T>> yearData = allData.stream().collect(Collectors.groupingBy(T::getYear));

```

### 二、条件筛选

#### 单条件筛选
```
List<T> filterList = appleList.stream().filter(a -> a.getName().equals("YC")).collect(Collectors.toList());

```

#### 多条件筛选
```
List<T> filterList = dayVoList.
                      stream().filter(a -> a.getYEAR().equals(item)).collect(Collectors.toList())
                      .stream().filter(a -> a.getPrice() != "0" && a.getPrice() != "0.0").collect(Collectors.toList());
 

```

### 三、List合并

#### 1.合并去重
```
List<String> result = Stream.of(Lists.newArrayList("A", "B", "C"), Lists.newArrayList("A", "B"))
  .flatMap(Collection::stream).distinct().collect(Collectors.toList());

```

#### 2.合并不去重
```
List<String> result = Stream.of(Lists.newArrayList("A", "B", "C"), Lists.newArrayList("A", "B"))
  .flatMap(Collection::stream).collect(Collectors.toList());

```

## 四、List排序

#### 1.倒序
```
 List<T> api_list = apiData
                .stream().sorted(Comparator.comparing(T::getID).reversed()).collect(Collectors.toList());


```

#### 2.正序
```
 List<T> api_list = apiData
                .stream().sorted(Comparator.comparing(T::getID).collect(Collectors.toList());

```

## 五、List 数据去重
```
 List<T> primaryFilterData = apiData.stream().collect(
                Collectors.collectingAndThen(
                        Collectors.toCollection(() -> new TreeSet<>(Comparator.comparing(T::getName))), ArrayList::new));


```
