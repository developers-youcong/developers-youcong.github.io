---
title: js之切换全屏和退出全屏实现
date: 2019-08-26 16:34:12
tags: "javascript"
---
应用场景:
比如很多网页游戏全屏之类的，或者是网上看小说等。
<!--more-->

核心代码:
```

//控制全屏
function enterfullscreen() { //进入全屏
	$("#fullscreen").html("退出全屏");
	var docElm = document.documentElement;
	//W3C
	if(docElm.requestFullscreen) {
		docElm.requestFullscreen();
	}
	//FireFox
	else if(docElm.mozRequestFullScreen) {
		docElm.mozRequestFullScreen();
	}
	//Chrome等
	else if(docElm.webkitRequestFullScreen) {
		docElm.webkitRequestFullScreen();
	}
	//IE11
	else if(elem.msRequestFullscreen) {
		elem.msRequestFullscreen();
	}
}

function exitfullscreen() { //退出全屏
	$("#fullscreen").html("切换全屏");
	if(document.exitFullscreen) {
		document.exitFullscreen();
	} else if(document.mozCancelFullScreen) {
		document.mozCancelFullScreen();
	} else if(document.webkitCancelFullScreen) {
		document.webkitCancelFullScreen();
	} else if(document.msExitFullscreen) {
		document.msExitFullscreen();
	}
}

var a = 0;
$('#fullscreen').on('click', function() {
	a++;
	a % 2 == 1 ? enterfullscreen() : exitfullscreen();
})

```


前端代码:
```
<a type="button" id="fullscreen" class="btn btn-default visible-lg visible-md"><i class="fa fa-refresh" aria-hidden="true"></i> 切换全屏</a>

```
