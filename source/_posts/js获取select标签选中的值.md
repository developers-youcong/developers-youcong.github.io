---
title: js获取select标签选中的值
date: 2019-04-03 19:51:30
tags: "javascript"
---

两种方式，原生js和jQuery。
特别是作为全栈开发工程师，学会使用框架固然重要，但是也要使用的基础上，由浅入深，搞懂原理，这样才能在技术进化迅速的时代立于不败之地。
举个例子说明以下，以我这篇文章为例[node.js之十大Web框架](https://www.cnblogs.com/youcong/p/10503099.html)，当你学会Node.js的语法，有过使用Node.js开发几个简单Demo的经验，你会发现这些并不难。但是如果你想深入的理解Node.js，比如VsCode就是在Electron+TypeScript等基础上研发出来，但是Electron 是基于 Chromium 和 Node.js，如果你对Node.js和Chromium毫不了解，自己深入到Electron，注定是要吃亏的。
<!--more-->

使用原生js获取select标签选中值，源码如下:
```
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>

</head>
<body>
 <form action="#">
 <select id="test">
 <option value="1">VsCode</option>
  <option value="2">Atorm</option>
 </select>
 <input type="button" value="测试" onclick="t1()">
 </form>
 <script>
 function t1(){
   var myselect=document.getElementById("test");
  var index=myselect.selectedIndex;
  alert(myselect.options[index].value)
 }

 </script>

</body>
</html>


```

使用jQuery获取select标签选中值，源码如下:
```
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>

</head>
<body>
 <form action="#">
 <select id="test">
 <option value="1">VsCode</option>
  <option value="2">Atorm</option>
 </select>
 <input type="button" value="测试" onclick="t1()">
 </form>
 <script src="jquery-1.8.0.min.js"></script>
 <script>
 function t1(){
   var options=$("#test option:selected");
   alert(options.val());//获取value
   alert(options.text());//获取文本
}
 </script>

</body>
</html>


```
