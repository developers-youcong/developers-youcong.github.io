---
title: jQuery之遍历
date: 2020-07-31 10:37:52
tags: "js"
---

jQuery针对不同的对象遍历的方式是不一样的。
<!--more-->
比分说对接API时，如果是下面这样的数据:
```
[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object]

```

这样的数据一看多个对象，无非就是数组，就需要使用for循环的方式进行遍历才能正常渲染，否则会出现拿不到对象里包含的具体值。

for循环遍历代码如下:
```
    var historyEvents = result.result;
                
    for (var i = 0; i < historyEvents.length; i++) {
        console.log(historyEvents[i].date)
    }


```

如果是这样的数据:
```
[object Object]

```
通过JSON.stringify(obj)解析，展示就是JSON数据。
面对这样的数据，直接使用$.each遍历的方法即可。
$.each循环遍历代码如下:
```
  $.each(posts, function (i, post) {
  });

```
