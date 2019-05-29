---
title: node.js如何引用其它js文件
date: 2019-03-07 19:50:02
tags: "Node.js"
---

以Java来说，比如要实现第三方存储，我可能需要导入对应的库，以maven为例，使用腾讯云或者七牛云、阿里云，我需要导入对应的maven依赖。
再比如，有些时候我们封装某个类，而那个类不在该包下，我们需要导包(就是指定那个类的路径，如果路径不对，则可能出现找不到这个类之类的错误，通常对应的IDE会提示错误)。

其实，node.js也是这样的。最近在开发node.js的时候，难免也会遇到需要引入其它js文件。今天我以一个简单示例来说一说node.js如何引用其它js文件。
<!--more-->
test01.js
```

function hello(){
	
	console.log("hello");
}

function hello2(){
	
	console.log("hello2");
}

module.exports = {hello,hello2}
```


test02.js
```
var test01 = require( "./test01" );

test01.hello();

test01.hello2();
```

通过命令行运行node test02.js 正常会分别输出hello、hello2。

require是什么意思呢？
其实就跟我们Java开发导包一样的意思，在Java中是import，其实node.js也可以import式导包。

那么node.js中的require和import导包有什么区别呢？
(1)require导包位置任意，而import导包必须在文件的最开始;
(2)遵循的规范不同，require/exports是CommonJS的一部分，而import/export是ES6的规范;
(3)出现时间不同，CommonJS作为node.js的规范，一直沿用至今，主要是因为npm善CommonJS的类库众多，以及CommonJS和ES6之间的差异，Node.js无法直接兼容ES6。所以现阶段require/exports仍然是必要且必须的;
(4)形式不同，require/exports的用法只有以下三种简单写法:
```
const fs = require('fs');
— — — — — — — — — — — — — — 
exports.fs = fs;
module.exports = fs;
```
而import/exports的写法就多种多样
```
import fs from 'fs';
import {default as fs} from 'fs';
import * as fs from 'fs';
import {readFile} from 'fs';
import {readFile as read} from 'fs';
import fs, {readFile} from 'fs';
— — — — — — — — — — — — — — — — — — — — 
export default fs;
export const fs;
export function readFile;
export {readFile, read};
export * from 'fs';
```

(5)本质上不同，主要体现:
a.CommonJS还是ES6 Module 输出都可以看成是一个具备多个属性或者方法的对象;
b.default是ES6 Module所独有的关键字，export default fs 输出默认的接口对象，import fs from 'fs'可直接导入这个对象;
c.ES6 Module 中导入模块的属性或者方法是强绑定的，包括基础类型，而CommonJS则普通的值传递或者引用传递;


本文参考链接如下所示:
node.js如何引用其它js文件:https://www.cnblogs.com/wuotto/p/9640312.html
关于require/import的区别:https://www.jianshu.com/p/fd39e16feb60