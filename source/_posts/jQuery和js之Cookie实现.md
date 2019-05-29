---
title: jQuery和js之Cookie实现
date: 2019-03-13 22:11:06
tags: "javascript"
---
Web开发者的朋友们基本上都知道，jQuery是对js的封装。今天之所以想讲解这个问题，主要是因为Cookie用的还是比较多，应用场景除了老生常谈的购物车，还有就是用户状态(以我之前开发的一个项目除了session和token外，还有一个加密cookie，双重保护，确保系统安全)。
<!--more-->

## 一、js中的Cookie增加、获取、删除操作


### 1.添加Cookie(保存Cookie)
```
function setCookie(c_name,value,expiredays){
    var cookieStr = "";
    var exdate=new Date();
    exdate.setDate(exdate.getDate()+expiredays);
    document.cookie = c_name+ "=" +escape(value)+
    ((expiredays==null) ? "" : "; expires="+exdate.toGMTString())+";path=/";
}//由于cookie存在域的概念，且在这里要不区分域，获取cookie的值，所以在这里使用的是统一的路径 path=/ ；
```

### 2.获取Cookie
```
function getCookie(c_name){
    if (document.cookie.length>0){ 
        console.log(document.cookie);
        c_start=document.cookie.indexOf(c_name + "=");
        if (c_start!=-1){ 
            c_start=c_start + c_name.length+1; 
            c_end=document.cookie.indexOf(";",c_start);
            if (c_end==-1) c_end=document.cookie.length;
            return unescape(document.cookie.substring(c_start,c_end));
        } 
    }
    return "";
}

```

### 3.删除Cookie

```
function delete_cookie( name, path, domain ) {
  if( get_cookie( name ) ) {
    document.cookie = name + "=" +
      ((path) ? ";path="+path:"")+
      ((domain)?";domain="+domain:"") +
      ";expires=Thu, 01 Jan 1970 00:00:01 GMT";
  }
}

```

## 二、jQuery如何操作Cookie

前提必须要有jQuery.min.js和jQuery.cookie.js。
jQuery.cookie.js下载:http://plugins.jquery.com/cookie/

### 1.jQuery添加Cookie
```
$.cookie('the_cookie', 'the_value', { expires: 7 });

```

### 2.jQuery获取Cookie
```
$.cookie('the_cookie');

```


### 3.jQuery删除Cookie
```
$.cookie('the_cookie', null); 

```


js和jQuery对比，两者效果明显，从中也能体现框架化繁为简的特性。


注意事项:
有的浏览器禁用Cookie会看不到Cookie信息，比如Google。上面的例子是没有问题的，如果你发现在你本地运行不出来，该导入的库也导入了还是没有效果，也不报错，这个时候你就需要看看是不是浏览器禁用Cookie了。

本文参考资料:
jQuery之操作Cookie:https://www.cnblogs.com/s313139232/p/7839037.html
js中Cookie操作:https://www.cnblogs.com/springlight/p/5953153.html
关于js操作Cookie(包含Cookie相关的基础知识):https://blog.csdn.net/web_yzm/article/details/81669772
