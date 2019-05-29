---
title: Node.js读取文件内容并返回值(非异步)
date: 2019-04-02 14:43:59
tags: "Node.js"
---
主要解决的问题的，以最近VsCode插件开发为例，每次请求都需要token,而vscode并不支持cookie这样的存储，所以就采用粗暴点办法，存到某个用户目录下并读取。

<!--more-->

源码如下:
```
var fs=require("fs");

 function getToken(isRelease) {
		//是否为正式版本，路径不一样
		if (isRelease) {
			const scriptSrc = path.dirname(__filename);
			const jsName = scriptSrc.split('\\');
			var i = jsName.length;
			var finpath = "";
			for (var j = 0; j < i - 3; j++) {
				if (j == 0) {
					finpath = jsName[j];
				} else {
					finpath = finpath + '\\' + jsName[j];
				}
			}
			finpath = finpath + '\\token.txt';

		} else { 
			finpath = 'D://Workspace//token//token.txt';
		}


		if (fs.existsSync(finpath)) { //判断是否存在该文件
			try {
				let result = fs.readFileSync(finpath);
				console.log(result.toString());



				return result.toString();
			
			} catch (e) {

			}

		}


	}
	
	console.log(getToken());

```
