---
title: TypeScript之Https通信
date: 2019-06-21 14:39:55
tags: "VsCode"
---

NetWorkRequest.ts(源代码如下)
<!--more-->
```

import * as https from "https";
import * as vscode from 'vscode';
import * as querystring from 'querystring';
export class NetWorkRequest {

    public static sendRequest(userCode: string) {

        vscode.window.showInformationMessage("userCode:" + userCode);

        var userId = userCode;
        var post_data = { userId: userId }
        var contents = querystring.stringify(post_data);

        var options = {
            hostname: "www.test.com",
            port: 443, //443
            path: "/test-web/api/sysUser/getUserCodeByInfo?" + contents,
            method: "POST",
            rejectUnauthorized: false,
            headers: {
                Accept: "*/*",
                "Accept-Encoding": "utf-8",
                "Accept-Language": "zh-CN,zh;q=0.8",
                Connection: "keep-alive",
                Host: "www.test.com"
            },

        };
        var mData = "";
        var req = https.request(options, function (res) {
            res.setEncoding('utf-8');
            res.on("data", function (d) {

                var data = JSON.parse(mData+d);
                console.log("============================================data======================================================:" + data);
               
            });

        });
        // req.write(contents);
        req.on("error", function (e) {

        });
        req.end();
    }

}



```

那么如何调用呢？

调用其实与Java调用很相似，基本上都是类名.方法。

如下调用:
```
import { NetWorkRequest } from './NetWorkRequest';
NetWorkRequest.sendRequest(userCode);
    

```

import 导入对应的模块(通常是某个ts文件)