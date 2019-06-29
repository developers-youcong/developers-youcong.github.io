---
title: javascript如何动态修改iframe的src
date: 2019-06-22 11:40:01
tags: "javascript"
---

为什么需要动态修改iframe的src?
一般情况我们使用iframe,其中的src通常是写死的，但是有些时候我们不希望它是死的src，而是一个活的src。
<!--more-->

示例代码如下:
```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>测试</title>
</head>
<body>
<iframe
width = "100%"
height = "800" id="iframe">
</iframe>
<script>
iframe.src = "https://www.baidu.com";
</script>
</body>
</html>

```