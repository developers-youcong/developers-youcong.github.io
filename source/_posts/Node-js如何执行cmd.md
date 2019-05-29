---
title: Node.js如何执行cmd
date: 2019-04-26 20:47:16
tags: "Node.js"
---

最近正好因业务的一个需求需要研究如何根据vscode的插件名来下载对应的插件以解决之前将插件打包上传到服务器上面导致的延迟问题(插件体积小还好说，如果体积过大，即便是压缩打成zip包，如果同一时刻很多人上传或下载，系统延迟将会非常严重)。
之前一直想不明白，找半天找不到要给URL可以下载，最后不经意间有了灵感转变一下思路搞定了。灵感是一个好东西。
本文主要讲Node.js如何执行cmd,应用场景除了我开头说的，其实还有很多，只有想不到，没有做不到。正如我们经理说的，现在基本上20%的技术可以解决80%的业务问题，这个时代，技术有点泛滥，换言之，技术产能过剩。
<!--more-->

## 一、下载node-cmd
```
npm install -g node-cmd

```


## 二、编写测试函数(index.js)
```
var nodeCmd = require('node-cmd');
 
function runCmdTest() {

			   var fileName = "ms-ceintl.vscode-language-pack-zh-hans";

			   console.log("fileNames:"+fileName);
			   
			   nodeCmd.get(
			 
					'code --install-extension '+fileName+' --extensions-dir="D:\1024Workspace\extension"',
			 
					function(err, data, stderr){
			 
						console.log(data);
			 
					}
			 
				);

		nodeCmd.run('code --install-extension '+fileName+' --extensions-dir="D:\1024Workspace\extension"');

}

console.log(runCmdTest());

```

参考资料如下:
[nodejs 运行CMD命令](https://blog.csdn.net/llzkkk12/article/details/78171750)