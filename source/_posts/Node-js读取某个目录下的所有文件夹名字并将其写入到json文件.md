---
title: Node.js读取某个目录下的所有文件夹名字并将其写入到json文件
date: 2019-03-19 14:08:12
tags: "Node.js"
---
针对解决的问题是，有些时候我们需要读取某个文件并将其写入到对应的json文件(xml文件也行，不过目前用json很多，json是主流)。
<!--more-->
源码如下:
index.js
```
var fs = require('fs');

let components = []
const files = fs.readdirSync('./')
files.forEach(function (item, index) {
    let stat = fs.lstatSync("./" + item)
    if (stat.isDirectory() === true) { 
      components.push(item)
    }
})

console.log(components);

let str = JSON.stringify(components)
 
 fs.writeFile('./extension.json',str,function(err){
 if (err) {res.status(500).send('Server is error...')}
})


```

控制台输出对应的数据:
```
[ 'file',
  'LearningDemo',
  'VsCode',
  'VsCode文件备份',
  'WeChatApp',
  '业务管理文档',
  '技术管理文档',
  '阿里云服务器备份' ]

```

参考资料如下:
nodejs写入json文件，格式化输出json的方法:http://www.cnblogs.com/threeEyes/p/10023827.html
利用nodejs对本地json文件进行增删改查:https://blog.csdn.net/zhaoxiang66/article/details/79894209