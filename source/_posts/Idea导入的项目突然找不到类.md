---
title: Idea导入的项目突然找不到类
date: 2022-12-25 12:04:28
tags: "IDEA"
---

前段时间使用Idea时不时突然出现依赖找不到、类找不到、编译变红之类的问题。
<!--more-->

而后通过搜索，参考网上一些朋友的解决方式，**归纳如下:**

- 1.通过菜单：File --> Invalidate Caches/Restart --> Invalidate Caches and Restart 进行重启。
- 2.删除工程最外层的 .idea 文件并重新导入。
- 3.检查Maven的配置是否为自己本地的Maven配置(有时候会替换为默认idea的)：File-->Settings。
- 4.Idea点击项目根目录，选 Open Module Settings，然后检查是否设置了SDK。
- 5.上述4种办法均不管用的话，重启电脑以后，重新打开Idea。


以上5种亲试有效！！！