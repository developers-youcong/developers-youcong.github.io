---
title: js截取get参数乱码问题之解决
date: 2019-07-31 15:33:46
tags: "javascript"
---
举个例子说一下:
```
http://wwww.yctech.com/blog/post?id=1

```
<!--more-->
像这样的话，通常通过如下代码直接截取不用做任何处理:
```
function getQueryString(name) {
    var result = window.location.search.match(new RegExp("[\?\&]" + name + "=([^\&]+)", "i"));
    if (result == null || result.length < 1) {
        return "";
    }
    return result[1];
}
```

但是当`http://wwww.yctech.com/blog/post?id=1` 变成`http://wwww.yctech.com/blog/post?id=挑战者`

这时，如果用getQueryString(name)方法截取的话，那么就会出现乱码，对于这种乱码的解决方式也很简单:
就是通过`encodeURI()`解决。

如:
```
var ids = getQueryString("id");//乱码
var id = encodeURL(ids);//处理乱码
alert(id);//弹出挑战者


```
参考资料如下:
[js传url中文参数乱码问题](https://www.cnblogs.com/TivonStone/p/3504922.html)