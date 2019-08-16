---
title: Node.js之删除文件夹(含递归删除)
date: 2019-07-20 12:10:11
tags: "Node.js"
---
应用场景:比如像Eclipse这样的IDE，右击项目，出现选项，点击选项中的删除，就可以删除这个项目及其下的子目录包含文件(使用electron开发的桌面端项目多少都会用到)。
<!--more-->
核心代码如下:
```
/**
	 *
	 * @param {*} url
	 */
	function deleteFolderRecursive(url) {
		var files = [];
		/**
		 * 判断给定的路径是否存在
		 */
		if (fs.existsSync(url)) {
			/**
			 * 返回文件和子目录的数组
			 */
			files = fs.readdirSync(url);
			files.forEach(function (file, index) {

				var curPath = path.join(url, file);
				/**
				 * fs.statSync同步读取文件夹文件，如果是文件夹，在重复触发函数
				 */
				if (fs.statSync(curPath).isDirectory()) { // recurse
					deleteFolderRecursive(curPath);

				} else {
					fs.unlinkSync(curPath);
				}
			});
			/**
			 * 清除文件夹
			 */
			fs.rmdirSync(url);
		} else {
			console.log("给定的路径不存在，请给出正确的路径");
		}
	}


```