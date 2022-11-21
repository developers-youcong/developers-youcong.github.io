---
title: npm问题之解决
date: 2022-02-25 18:58:15
tags: "npm"
---

## 错误信息1
<!--more-->

**错误信息1:**
```
gyp ERR! configure error  gyp ERR! stack Error: EACCES: permission denied,.....build

```

**错误信息1解决方案:**
```
npm install --unsafe-perm

```

**错误信息1原因分析:**
使用了root用户在对应的Vue.js项目执行npm install导致的，如果你想要使用root项目成功完成npm install或npm run build:prod的话，需要执行上面的npm安装命令。通常不建议使用root用户。

如果一定要使用root用户，还可以通过如下命令解决这个问题:
```
npm config set user 0
npm config set unsafe-perm true

```

## 错误信息2

**错误信息2:**
```
fatal: unable to access ‘https://github.com/nhn/raphael.git/‘: OpenSSL SSL_connect: Connection was

```

**错误信息2解决方案(1):**
更换npm源
```
npm config set registry https://registry.npm.taobao.org

```
**错误信息2解决方案(2):**
```
git config --global --unset http.proxy 
git config --global --unset https.proxy

```

解决方案(1)不行的话就用解决方案(2)


**错误信息2原因分析:**
可能与npm源有关，也可能与代理设置有关。