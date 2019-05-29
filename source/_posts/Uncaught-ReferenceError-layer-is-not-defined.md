---
title: 'Uncaught ReferenceError: layer is not defined'
date: 2019-03-19 20:35:10
tags: "javascript"
---
错误详细信息,如下:
```
Uncaught ReferenceError: layer is not defined'

```

关键词就是not defined 未定义，那么解决方案就是给它定义。
<!--more-->
原来的问题代码如下:
```
 layer.confirm('确认要退出吗？', {
            btn : [ '确定', '取消' ]//按钮
        }, function(index) {
        	
            layer.close(index);
            //此处请求后台程序，下方是成功后的前台处理……
            var index = layer.load(0,{shade: [0.7, '#393D49']}, {shadeClose: true}); //0代表加载的风格，支持0-2
            
            delete_cookie("userId", "/");

	        window.location.href = "index.html";
	
        });

```

这样在我的博客首页是可以生效的，不会出现未定义。但是当我将其抽象为一个函数的时候，其它地方就不行了。

通过声明定义后，代码就变成如下:
```
 layui.use('layer',function () { 
        	
        layer.confirm('确认要退出吗？', {
            btn : [ '确定', '取消' ]//按钮
        }, function(index) {
        	
            layer.close(index);
            //此处请求后台程序，下方是成功后的前台处理……
            var index = layer.load(0,{shade: [0.7, '#393D49']}, {shadeClose: true}); //0代表加载的风格，支持0-2
            
            delete_cookie("userId", "/");

	        window.location.href = "index.html";
	
        });
        
        });

```

当时我在想引入layer.js来解决这个问题，但是仔细一看这并不是问题的关键所在。
解决问题，在于更好的理解问题，当然了，把握关键词也是很重要的(事半功倍)。




