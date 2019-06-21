---
title: layui如何自定义弹出层关闭事件
date: 2019-06-17 20:42:15
tags: "javascript"
---
再某些业务场景下，我们需要自定义弹出层关闭事件，代码示例如下:
<!--more-->
```
layui.use('layer', function () {

		var layer = layui.layer;

		layer.open({
			skin: 'demo-class',
			type: 1,
			title: '登录',
			area: ['600px', '700px'],
			closeBtn :0,
			content: $('.login'), //这里content是一个普通的String
			cancel: function () {
				vscode.postMessage({
					command: 'close'
				});
			}
		});
	});


```
