---
title: layui之日期和时间组件
date: 2019-04-05 23:11:01
tags: "javascript"
---
参考文档:https://www.layui.com/doc/modules/laydate.html
代码片段如下:
```
			layui.use('laydate', function(){
					  var laydate = layui.laydate;

					  laydate.render({
					    elem: '#createDate', // 指定元素
					    type:'datetime'
					  });
					});

```
<!--more-->
效果图如下:
![](./layui之日期和时间组件/date01.png)


其中type默认值为date(年月日)，有如下几个可选值:

![](./layui之日期和时间组件/date02.png)