---
title: VsCode插件与Node.js交互通信
date: 2019-06-05 15:15:48
tags: "VsCode"
---
首先关于VsCode插件通信，如果不明白的可以参考我的这篇博客[VsCode插件开发之插件初步通信](https://www.cnblogs.com/youcong/p/10294758.html)

如果需要详细例子的话，可以参考[VsCode插件开发](https://github.com/developers-youcong/vscode-extension-dev)

现在又有一个新的需求是，VsCode插件可以通过jQuery的方式/或者引入某种前端通信框架实现与后台交互。但是针对之前某个需求，需求描述:用户登录后在本地某盘创建特定的文件夹。通常像创建特定文件夹的话，一般都是后端语言实现。而我当时编写的这个插件是用JavaScript，JavaScript是不能读写文件的，当然了，有些朋友可能会说可以使用ActiveXObject，但是这个ActiveXObject有局限性，它仅仅只能支持IE浏览器，而不能支持像Google Chrome和火狐这样的通用性广的浏览器。

为了解决这个需求，我决定结合node.js解决这个问题。

首先明确一点，vscode插件开发，不管是使用JavaScript还是TypeScript，通常由于本地调试的需求，都需要安装对应库，而管理这个库，通常使用npm或yarn。由此我们便可知，我们直接可以在该插件中编写node.js相关的代码。
<!--more-->

实现需求的步骤如下:

## 引入vscode相关的库(因为要调用消息传递命令)

```

const testMode = false; // 为true时可以在浏览器打开不报错
// vscode webview 网页和普通网页的唯一区别：多了一个acquireVsCodeApi方法
const vscode = testMode ? {} : acquireVsCodeApi();
const callbacks = {};

/**
 * 调用vscode原生api
 * @param data 可以是类似 {cmd: 'xxx', param1: 'xxx'}，也可以直接是 cmd 字符串
 * @param cb 可选的回调函数
 */
function callVscode(data, cb) {
    if (typeof data === 'string') {
        data = { cmd: data };
    }
    if (cb) {
        // 时间戳加上5位随机数
        const cbid = Date.now() + '' + Math.round(Math.random() * 100000);
        callbacks[cbid] = cb;
        data.cbid = cbid;
    }
    vscode.postMessage(data);
}

```


## 消息传到node.js
```

	vscode.postMessage({
		command: 'login',
		text: nickName
	});

```

那么有朋友会问，那么node.js是如何接收它的?

```
module.exports = function (context) {

	var interval = null;
	var i = 0;
	var flag = false;

	context.subscriptions.push(vscode.commands.registerCommand('extension.demo.showWelcome', function (uri) {

		if (flag) {
			return;
		}

		const panel = vscode.window.createWebviewPanel(
			'testWelcome', // viewType
			"功能页", // 视图标题
			vscode.ViewColumn.One, // 显示在编辑器的哪个部位
			{
				enableScripts: true, // 启用JS，默认禁用
			}
		);

		flag = true;

		panel.onDidDispose(
			() => {
				flag = false;
			},
			null, context.subscriptions);

		let global = {
			panel
		};

		panel.webview.html = getWebViewContent(context, 'src/view/custom-welcome.html');

		//创建文件
		panel.webview.onDidReceiveMessage(message => {

			switch (message.command) {

				
				case 'login':

					create(message.text, true);

					break;

				case 'time':

					start();

					break;

				case 'showCourseList':

					vscode.commands.executeCommand('extension.demo.showCourseList', message.text);

					break;

				case 'closeTime':
					stop();

					break;

				case 'themeColor':

					var name = getTheme();

					panel.webview.postMessage({
						command: 'refactor',
						text: name
					});

					break;

				case 'readToken':

					var name = getToken();
					console.log("-------------go go go go go go go ------------------------:" + name);
					panel.webview.postMessage({
						command: 'checkToken',
						text: name
					});

					break;
				case 'storeUserId':
					console.log("----------------store UserId-----------------:" + message.text);

					storeUserId(message.text);

					break;

				case 'getUserId':
					var userId = getUserID();

					console.log("----------------Get UserId-----------------:" + userId);

					panel.webview.postMessage({
						command: 'readUserId',
						text: userId
					});



					break;
				case 'storageToken':

					storeToken(message.text);

					break;
				case 'deleteToken':

					deleteToken(message.text);

					break;
				case 'readUploadFilePath':

					console.log("------------------------ readUploadFilePath -----------:" + message.text);

					var content = readExtensionFile(message.text);

					panel.webview.postMessage({
						command: 'uploadConfig',
						text: content
					});

					break;

				case 'downloadExtensionFile':

				      downloadExtensionFile(message.text);

					break;


			}

		}, undefined, context.subscriptions);




	}));

```

通过message.command我们就可以获取对应的command，根据command对应的字符走对应的case。这就是前端JavaScript与Node.js的通信。



## Node.js如何响应JavaScript的通信(相当于打电话，我打电话给你，不能仅仅是我在说，也需要你的响应(回答))

Node.js响应JavaScript通信代码如下(发送消息给window.addEventListener与前端JavaScript发送消息给Node.js本质上是一样的)
```


	window.addEventListener('message', event => {

		const message = event.data; // The JSON data our extension sent

		switch (message.command) {

			case 'refactor':
				console.log("自定义背景颜色 custome background color:" + message.text);

				if (message.text == "light") {
					document.body.style.backgroundColor = "#FFFFFF";
				} else {
					document.body.style.backgroundColor = "#333333";
				}
				break;

			case 'checkToken':

				console.log("-------------------checkToken----------------------------:" + message.text);

				if (message.text == null || message.text == "") {
					Login()
				} else {

					$("#exit").show();
					$("#settings-sync").show();
					$("#upload_settings").show();
					$("#token").val(message.text);
					readUploadFilePath("D://1024Workspace//extension//");

				}

				break;

			case 'readUserId':

				console.log("=====================readUserID=========================:" + message.text);

				$("#userId").val(message.text);
				checkPermissions(message.text);

				break;

			case 'uploadConfig':
				console.log("========================Upload Config ======================:" + message.text);

				$("#uploadExtensionContent").val(message.text);

				break;

		}
	});

```

window.addEventListener在此相当于监听全局。

通过message.text我们可以获取node.js响应给前端JavaScript的文本消息或者是json数据。

上述说的两点用官方的话语表示如下:
(1)JavaScript与Node.js通信;
(2)Node.js与JavaScript通信;
