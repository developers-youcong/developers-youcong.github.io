---
title: docsify建站如何通过域名访问
date: 2023-10-20 02:32:23
tags: ["docisy","Github","域名访问","网站"]
---

原framework.youcongtech.com服务器以及域名需用作其它用途，但是为了保障yc-framework官网的正常访问，特别将其与
<!--more-->
[youcongtech.com](https://youcongtech.com)网站做了关联。具体参考方案：
[docsify部署](https://docsify.js.org/#/zh-cn/deploy)

原来[developers-youcong.github.io](https://developers-youcong.github.io)(现在的[youcongtech.com](https://youcongtech.com))已经做好了域名映射，这个时候我只需确保yc-framework-docs这个仓库是公开的即可(已经将docs目录下主要核心文件上传)。上传完毕后，就能通过
[https://youcongtech.com/yc-framework-docs](https://youcongtech.com/yc-framework-docs)正常访问。但是注意了，需要设置好访问分支，不然容易出问题，亲试有效。