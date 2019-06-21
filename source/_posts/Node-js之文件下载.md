---
title: Node.js之文件下载
date: 2019-05-31 16:06:33
tags: "Node.js"
---

Node.js之文件下载，主要最近解决我的一个需求。

需求描述:
如何将腾讯云上传的文件存储到本地某个目录下，如果用js来实现，纯JavaScript没有这样的功能(也许有)，正好我这个项目用node.js比较多，正好可以利用node.js丰富的API实现该功能。
<!--more-->
如下示例代码，演示下载远程文件:

源码如下(download.js):
```

//下载参数
var http = require("http");
var fs = require("fs");
var path = require("path");
var downFlag = false;
var downUrl = '';
var downFileName = '';

/**
 * 下载回调
 */
function getHttpReqCallback (imgSrc, dirName, fileName) {

    var callback = function(res) {
        console.log("request: " + imgSrc + " return status: " + res.statusCode);
        var contentLength = parseInt(res.headers['content-length']);
        
        var downLength = 0;
    
        var out = fs.createWriteStream(dirName + "/" + fileName);
        res.on('data', function (chunk) {
            
            downLength += chunk.length;
            var progress =  Math.floor(downLength*100 / contentLength);
            var str = "下载："+ progress +"%";
            console.log(str);
            
            //写文件
            out.write(chunk, function () {
                //console.log(chunk.length);
                
            });
            
        });
        res.on('end', function() {
            downFlag = false;
            console.log("end downloading " + imgSrc);
            if (isNaN(contentLength)) {
                console.log(imgSrc + " content length error");
                return;
            }
            if (downLength < contentLength) {
                console.log(imgSrc + " download error, try again");
                return;
            }
        });
    };

    return callback;
}

/**
 * 下载开始
 */
function startDownloadTask (imgSrc, dirName,fileName) {
    console.log("start downloading " + imgSrc);
    var req = http.request(imgSrc, getHttpReqCallback(imgSrc, dirName, fileName));
    req.on('error', function(e){
        console.log("request " + imgSrc + " error, try again");
    });
    req.end();
}

startDownloadTask('http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.41/bin/apache-tomcat-8.5.41.tar.gz','D://1024Workspace//extension','apache-tomcat-8.5.41.tar.gz');

//startDownloadTask('下载地址','本地存储路径','文件名');


```

代码经过测试，没有问题。

本文主要参考资料如下:
Node.js文件下载