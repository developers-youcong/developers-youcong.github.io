---
title: Input byte array has incorrect ending byte at 24(一次密钥问题解决)
date: 2022-04-04 15:22:26
tags: "Java"
---

## 1.背景
安卓加密某个参数并传至后端解密
<!--more-->

## 2.错误信息
```
Input byte array has incorrect ending byte at 24

```

## 3.错误原因
存在\r|\n之类的特殊字符需要全局替换为空的字符串，否则后端再解析过程中会出现报错导致无法解密。

## 4。解决办法
```
str.replaceAll("\r|\n", "");

```
