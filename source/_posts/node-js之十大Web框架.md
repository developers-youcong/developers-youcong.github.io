---
title: node.js之十大Web框架
date: 2019-03-07 21:14:41
tags: "Node.js"
---
之前接触过Node.js是因为好奇大前端越来越能干了，连我后台的饭碗都要抢了，太嚣张了，于是我想打压打压它，然后就这样接触它了。
再到后来是因为Settings-Sync插件二次开发，我需要用node.js造一个mock server，而当时在开源项目上找到一个模拟github rest api的node.js服务端程序，然后我就在此基础上开发。从工作中学习有应用场景有目的性，果然还是学的要快很多。

今天之所以要说一说node.js的十大Web框架，主要是觉得以后针对VsCode开发或者是我自己的项目，用Node.js可能会比较多，比如我将我的博客系统一步一步完善，我想尝试微服务的很多种实践方式，其实很多企业用微服务，还有一个原因就是不受编程语言的制约。

大前提:框架无优劣之分，只有在某个应用场景是否更适合和更好。
<!--more-->
## 一、Node.js开发框架Sail.js

Sail.js官网地址为:https://sailsjs.com/
github地址:https://github.com/balderdashy/sails

Sails可以轻松构建自定义的企业级Node.js应用程序

在几周内而不是几个月内构建实用的，生产就绪的Node.js应用程序。Sails是Node.js最流行的MVC框架，旨在模拟Ruby on Rails等框架的熟悉MVC模式，但支持现代应用程序的需求:具有可扩展性，面向服务的体系结构的数据驱动API。

优点如下:
(1)100%JavaScript
在Sails自上构建意味着你的应用程序完全使用JavaScript编写，从这里可以看出浏览器兼容性良好;
(2)任何数据库
Sails捆绑了一个功能强大的ORM，即Waterline，它提供了一个简单的数据访问层，无论你使用什么数据库，它都能正常工作;
(3)自动生成的Rest API
Sails附带的蓝图有助于在不编写任何代码的情况下快速启动应用程序的后端;
(4)前端不可知
Sails与任何前端兼容:Angular、React、IOS、Android、Windows Phone，自定义硬件或其他完全兼容;
(5)轻松WebSocket集成
由于Sails会为您转换传入的套接字消息，因此它们会自动与Sails应用中的每个路由兼容;
(6)专业支持
Sails提供商业支持，以加速开发并确保代码职工的最佳实践;


## 二、Node.js服务端框架 Hapi.js

官网地址为:https://hapijs.com/

如何创建一个Hapi.js?请参考如下步骤(均来自官网示例)：
1.初始化

```
npm init
```
2.安装库

```
npm install hapi --save

```
3.编写js

```
'use strict';

const Hapi=require('hapi');

// Create a server with a host and port
const server=Hapi.server({
    host:'localhost',
    port:8000
});

// Add the route
server.route({
    method:'GET',
    path:'/hello',
    handler:function(request,h) {

        return'hello world';
    }
});

// Start the server
const start =  async function() {

    try {
        await server.start();
    }
    catch (err) {
        console.log(err);
        process.exit(1);
    }

    console.log('Server running at:', server.info.uri);
};

start();

```
4.运行

```
npm start

```

## 三、Node.js 高性能封装 Express.js
关于这个可以参考我的这篇博客:https://developers-youcong.github.io/2019/02/22/node-js%E4%B9%8Bexpress%E6%A1%86%E6%9E%B6/

## 四、Node.js Web框架 Kraken.js
Kraken 基于 express 构建，实现对环境变量的感知、动态配置、高级中间件和应用生命周期的事件通知。

官网地址为:http://krakenjs.com/

示例例子:
```
'use strict';

var express = require('express'),
    kraken = require('kraken-js');

var app = express();
app.use(kraken());
app.listen(8000);
```

## 五、Web应用构建平台 Meteor
Meteor是一组新的技术用于构建高质量的Web应用，提供很多现成的包，可直接在浏览器或者云平台运行。
官网地址为:https://www.meteor.com/

优点如下:
(1)使用更少的代码运送更多
由于集成的JS堆栈从数据库扩展到最终用户的屏幕，因此可以在10行中完成1000行.

(2)为任何设备构建应用程序
无论是针对Web、IOS、Android还是桌面进行开发，都使用相同的代码。热门推送新功能，无需应用商店批准或者强制用户下载新的原生应用.

(3)集成已有的技术
使用流行的框架和工具，开箱即用。专注于构建功能，而不是自己将不同的组件连接在一起。


## 六、全栈JavaScript 开发架构 Mean.js

官网地址为:http://meanjs.org/

1.什么是Mean.js

MEAN.JS是一个全栈JavaScript的解决方案，可帮助您使用MongoDB、Express、Angular和Node.js构建快速，健壮且可维护的生产Web应用程序.

2.为何选择Mean.js
Mean.js将帮助你入门并避免无用的笨拙工作和常见陷阱，同时保持你的应用程序井然有序。我们目标是创建和维护一个简单易读的开源解决方案，你可以使用它并信任它。


3.入门
入门请参考官方文档:http://meanjs.org/docs.html

## 七、Node.js的Web框架 Koa.js
Koa是下一代的Node.js的Web框架。由Express团队设计。旨在提供一个更小型、更富有表现力、更可靠的Web应用和API的开发基础。
Koa可以通过生成器摆脱回调，极大地改进错误处理。Koa核心不绑定任何中间件，但提供了优雅的一组可以快速愉悦地编写服务器应用的方法。
关于koa可参考:https://www.npmjs.com/package/koa


## 八、Node.js CMS 和 Web 应用程序平台 KeystoneJS
KeystoneJS，以 Express 和 MongoDB 为基础搭建的 Node.js CMS 和 Web 应用程序平台。

官网地址为:https://keystonejs.com/

具有以下特性：

Express.js 和 MongoDB：Keystone 会为你配置 express（node.js 上的 Web 服务器），用 Mongoose（领先的 ODM 包）连接你的 MongoDB 数据库

动态路由：Keystone 从设置 MV* 程序的最佳实践入手，让你管理模板、视图和路由变得更容易

数据库域：ID、String、Boolean、Date 和 Number 是数据库的构件。Keystone 以它们为基础实现了在现实工作中更实用的域类型，比如 name、email、password、address、image 和 relationship (及其它)

自动生成管理员界面：不管你在搭建应用程序，或者在生产环境中作为数据库内容管理系统时是否用它，Keystone 的管理员界面都能节省你的时间，让你管理数据更容易

编码更简单：有时即便做的事情简单，异步代码也会变得复杂。Keystone让简单的事情（比如在视图中显示之前加载数据）保持简单

表单处理：要验证表单、上次图片或用一行代码更新数据库？基于你已经定义的数据模型，Keystone 可以做到

会话管理：Keystone 自带了会话管理和认证功能，包括密码域的自动加密

发送 Email：借助 Keystone，你的应用程序可以轻松地设置、预览和发送基于模板的 email。它还集成了 Mandrill (Mailchimp 卓越的事务性 email 发送服务)

## 九、Node.js 框架组件 flatiron.js
flatiron 是一款 Node.js 和浏览器的框架组件，是一款构建现代化 web 应用适应性很强的框架。flatiron 提供比 Rails 类组件有更丰富配置的框架组件，允许开发者自己添加他们想要的功能组件。
可参考地址为:https://www.npmjs.com/package/flatiron


## 十、基于 Node.js 的 API 框架 LoopBack
LoopBack 是基于 Node.js 的一个开源的 API 框架，可以让 Node.js 应用方便的跟各种设备通过 API 进行互联。
可参考地址为:https://loopback.io/

本文主要参考除引入的官网外还参考[Node.js十大Web框架](https://my.oschina.net/editorial-story/blog/956498)
