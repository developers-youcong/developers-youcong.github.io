---
title: 解决js地址栏中传递中文乱码的问题
date: 2020-07-05 10:21:07
tags: "javascript"
---
问题背景:
地址栏传参有中文，中文截取后出现乱码问题。
<!--more-->
问题代码:
```
function getQueryString(name)
{
     var reg = new RegExp("(^|&)"+ name +"=([^&]*)(&|$)");
     var r = window.location.search.substr(1).match(reg);
     if(r!=null)return  unescape(r[2]); return null;
}

```

解决问题代码:
```
function getUrlParam(key) {
    // 获取参数
    var url = window.location.search;
    // 正则筛选地址栏
    var reg = new RegExp("(^|&)" + key + "=([^&]*)(&|$)");
    // 匹配目标参数
    var result = url.substr(1).match(reg);
    //返回参数值
    return result ? decodeURIComponent(result[2]) : null;
}

```

参考解决问题链接:
[js地址栏获取参数的方法，解决中文乱码问题，能支持中文参数](https://www.cnblogs.com/jorzen1984/p/6632918.html)