---
title: ajax之解析xml
date: 2019-11-26 16:22:11
tags: "javascript"
---

今天之所以要分享这个例子，也是因为最近做博客开发，将第三方博客相关文章迁移到我自己的平台来。

通常迁移到自己的平台上有这么几种方式(以博客园为例):
1.导出xml，使用java程序遍历读取并存入对应的数据表;
2.博客园有自己公开的API，可以通过请求API拿到对应的xml数据，插入数据表;
3.使用爬虫技术;

上述三种我都尝试过，今天我要说的是其中2，也就是根据博客园API拿到xml数据。

今天不说后端，后端我已经将博客园API做了一定的封装，今天主要说ajax如何解析xml，并获得想要的数据。
<!--more-->
前端代码:
```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8">
  <title>blog-analyze-xml-example</title>
</head>

<body>
<script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
<script>
$.ajax({
	type: "GET",
	url: "http://localhost:2019/blog-web/cnblogs/get48HoursTopViewPosts/20",
	dataType: "xml",
	success: function (ResponseText) {
		alert(ResponseText);
	
		$(ResponseText).find('author').each(function () {
			var name = $(this).find('name').text();
			
			console.log("name:"+name);
			
		})
	
	}
});

</script>

</body>

</html>

```

这段代码关键就是:
```

		$(ResponseText).find('author').each(function () {
			var name = $(this).find('name').text();
			
			console.log("name:"+name);
			
		})

```

xml是一个文档树形结构数据，通过find()找到你想要的节点，拿到对应的值，这时你想做什么就做什么。
