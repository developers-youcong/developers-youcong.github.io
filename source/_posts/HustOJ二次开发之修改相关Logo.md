---
title: HustOJ二次开发之修改相关Logo
date: 2019-08-08 18:27:48
tags: "HustOJ"
---

比如将如图中的HUSTOJ进行修改:
![](HustOJ二次开发之修改相关Logo/01.png)

在Linux上修改，通过关键字搜索，会获取如下两个重要文件，找到都有的文字进行修改即可:
```
grep -rn "HUSTOJ" *
cd /home/judge/src/web
vim include/db_info.inc.php 修改标题
vim template/bs3/js.php 修改底部

```
<!--more-->