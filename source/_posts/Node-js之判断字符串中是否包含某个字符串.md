---
title: Node.js之判断字符串中是否包含某个字符串
date: 2019-07-17 15:02:26
tags: "Node.js"
---

server.txt内容如下:
```
阿里云服务器

```

关于应用场景，就不多说了，字符串不管是前端开发人员，还是后端开发人员，都是要与其经常打交道的。

test.js(node.js代码，只要被本地装了node.js环境，直接可通过node test.js运行看效果):
<!--more-->
```
var fs = require("fs");

var result = fs.readFileSync("./server.txt");

console.log("result:"+result);

if(result.indexOf("阿里云服务器") != -1){
	
	console.log("ok");
} else{
	
	console.log("no");
	
}
//indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。如果要检索的字符串值没有出现，则该方法返回 -1。


if(result.toString().search("1") != -1){
	console.log("ok");
	
}else{
	console.log("no");
}
//search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。如果没有找到任何匹配的子串，则返回 -1。



var reg = RegExp(/阿里云/);
if(result.toString().match(reg)){
    console.log("ok");      
}else{
	console.log("no");
	
}
//match() 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。



var reg = RegExp(/阿里云/);
if(reg.test(result)){
	
	console.log("ok");
}else{
	console.log("no");
	
}

//test() 方法用于检索字符串中指定的值。返回 true 或 false。



var reg = RegExp(/阿里云/);
if(reg.exec(result)){	
   	console.log("ok");      
}else{
	console.log("no");
}
//exec() 方法用于检索字符串中的正则表达式的匹配。返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。



```

本文主要参考资料如下:
[js 判断字符串中是否包含某个字符串](https://blog.csdn.net/qq_41033913/article/details/90754507)