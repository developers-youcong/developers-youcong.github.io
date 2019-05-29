---
title: node.js读写文件
date: 2019-03-04 21:32:24
tags: "Node.js"
---
关于node.js的读写操作，应用场景有很多。
比如其中这样的一个场景，如何获取全局的token。
这就涉及到写和读操作了。
<!--more-->
写操作:
```
var fs = require("fs");

function storeToken(token){

	fs.writeFile('D://youcongtech//token.txt',token.slice(10),'utf8',function(error){

		if(error){
			console.log(error);
			return false;
		}
		console.log('write success');

	});
}
```

读操作:
```
var fs = require("fs");

function readToken(){


	fs.readFile('D://youcongtech//token.txt', function (err, data) {
		if (err) {
			return console.error(err);
		}
		console.log(data.toString());
	});

}


```

通过上述两个示例代码，就可以达到存取token的目的，这样一来就不必担心如何获取token问题。
当然了，问题的解决方式不止这一个，其实还可以用redis来存储token。
