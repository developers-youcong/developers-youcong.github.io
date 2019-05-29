---
title: layui之确认框
date: 2019-04-03 12:30:30
tags: "javascript"
---

要使用layui的确认框，需要导入layui的两个库，分别为layer.js和layer.css，除此之外layui.js和layui.css的库也是要导入，这个请注意。
所有说你需要分别导入四个库layer.js、layer.css、layui.js、layui.css，四个库多少也会占用带宽，这时你可以使用cdn或者将其放到nginx做缓存也行。
<!--more-->
确认框源码如下:
```
	        layui.use('layer',function () { 
	        	
	            layer.confirm('确认要删除吗？', {
	                btn : [ '确定', '取消' ]//按钮
	            }, function(index) {
	            	
	                layer.close(index);
	                //此处请求后台程序，下方是成功后的前台处理……
	                var index = layer.load(0,{shade: [0.7, '#393D49']}, {shadeClose: true}); //0代表加载的风格，支持0-2

	            });
	            
	            });

```

应用场景:
(1)所有的确认框操作，比如是否删除这样的;
(2)退出功能;