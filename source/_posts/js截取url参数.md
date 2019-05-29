---
title: js截取url参数
date: 2019-03-16 15:24:33
tags: "javascript"
---

举例说明，比如http://localhost:2019/blog/getCommentListInfo?postId=1
如何获取postId=1这个参数值呢？
<!--more-->
很简单通过下面代码即可获取，如:
```
window.onload = function() {

	var postId = getUrlParms("postId");

	getByIdCommentInfo(postId);
}

//获取地址栏参数，name:参数名称
function getUrlParms(name) {
	var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
	var r = window.location.search.substr(1).match(reg);
	if(r != null)
		return unescape(r[2]);
	return null;
}

```

参考资料:
js获取url传递参数，js获取url?号后面的参数:https://www.cnblogs.com/karila/p/5991340.html
