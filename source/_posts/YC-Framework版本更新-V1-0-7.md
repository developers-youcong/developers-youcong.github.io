---
title: 'YC-Framework版本更新:V1.0.7'
date: 2022-05-23 21:36:38
tags: "YC-Framework"
---

[分布式微服务框架:YC-Framework](https://youcongtech.com/2021/12/04/%E6%88%91%E7%9A%84%E5%88%86%E5%B8%83%E5%BC%8F%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%A1%86%E6%9E%B6-YC-Framework/)版本更新V1.0.7！！！

本文主要内容:

- V1.0.7版本更新主要内容
- V1.0.7版本更新主要内容介绍

<!--more-->

## 一、V1.0.7版本更新主要内容
- 1.增加自动化打包脚本 bash/shell；
- 2.优化Feign通信；
- 3.新增yc-ui(基于vue-element-admin改造)；
- 4.Nacos升级1.4.3；
- 5.项目打包构建问题修复；
- 6.yc-xjar微服务化(支持多个jar同时自动化加密）；
- 7.Sa-Token升级1.30.0；
- 8.Knife4j升级2.0.9；
- 9.yc-common-mp模块问题处理；
- 10.官网增加安全证书。

目前Github和Gitee已基本实现同步了，方便国内外朋友进行相应的交流。

官方网站:
http://framework.youcongtech.com/

GitHub源代码地址:
https://github.com/developers-youcong/yc-framework

Gitee源代码地址:
https://gitee.com/developers-youcong/yc-framework

历史版本查看:
https://github.com/developers-youcong/yc-framework/releases

欢迎大家star或fork[分布式微服务框架YC-Framework](https://youcongtech.com/2021/12/04/%E6%88%91%E7%9A%84%E5%88%86%E5%B8%83%E5%BC%8F%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%A1%86%E6%9E%B6-YC-Framework/)(star或fork是对开源项目的最好支持)！！！

## 二、V1.0.7版本更新主要内容介绍

### 1.增加自动化打包脚本
执行根目录下的deploy.sh，即可实现自动化打包并上传指定目录(对应的指定目录执行上传命令，就能将jar包上传至对应的服务器，并能完成部署)：
```
bash deploy.sh

```

### 2.优化Feign通信
采用性能较好的OkHttp！！！

### 3.新增yc-ui(基于vue-element-admin改造)
基于vue-element-admin二次开发！！！

### 4.Nacos升级1.4.3
原有的1.3.x版本存在一些问题，故升级Nacos官网稳定版1.4.3！！！

### 5.项目打包构建问题修复
部分代码以及模块导致打包失败问题修复！！！

### 6.yc-xjar微服务化(支持多个jar同时自动化加密）
针对有加密部署微服务的需求！！！

### 7.Sa-Token升级1.30.0
增加了更多的特性、修复了更多的问题等！！！

### 8.Knife4j升级2.0.9
解决了 issue 150+！！！

### 9.yc-common-mp模块问题处理
去除冗余代码，通用配置类抽取。


### 10.官网增加安全证书
官网配置https！！！