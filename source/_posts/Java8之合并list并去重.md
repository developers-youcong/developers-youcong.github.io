---
title: Java8之合并list并去重
date: 2020-09-09 20:44:32
tags: "Java"
---

核心代码如下:
```
List<String> result = Stream.of(Lists.newArrayList("A", "B", "C"), Lists.newArrayList("A", "B"))
  .flatMap(Collection::stream).distinct().collect(Collectors.toList());

```

最终的结果输出是A B C。

应用场景:
有些时候我们需要合并两个返回类型相同的结果集，就可以用这个，不必SQL查询合并结果。