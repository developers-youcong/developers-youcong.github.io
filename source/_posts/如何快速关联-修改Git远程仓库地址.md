---
title: 如何快速关联/修改Git远程仓库地址
date: 2019-04-30 11:27:54
tags: "Git"
---

## 如何快速关联/修改Git远程仓库地址?按照如下步骤即可快速实现关联/修改Git远程仓库地址:

#### 删除本地仓库当前关联的无效远程地址，再为本地仓库添加新的远程仓库地址
```
git remote -v //查看git对应的远程仓库地址
git remote rm origin //删除关联对应的远程仓库地址
git remote -v //查看是否删除成功，如果没有任何返回结果，表示OK
git remote add origin https://github.com/developers-youcong/Metronic_Template.git //重新关联git远程仓库地址
```

其实不仅仅上述这一种方式，还有如下几种方式:
<!--more-->
#### 直接修改本地仓库所关联的远程仓库的地址
```
git remote  //查看远程仓库名称：origin 
git remote get-url origin //查看远程仓库地址
git remote set-url origin https://github.com/developers-youcong/Metronic_Template.git  ( 如果未设置ssh-key，此处仓库地址为 http://... 开头)

```

#### 修改 .git 配置文件
```
cd .git  //进入.git目录
vim config  //修改config配置文件，快速找到remote "origin"下面的url并替换即可实现快速关联和修改

```


#### 如何快速关联/修改Git远程仓库地址的应用场景有哪些?
(1)同步开源项目(以VsCode为例，我针对其进行二次开发，但是其版本总是在更新为了保持与其一致，我就得用到修改git远程仓库同步其最新代码);
(2)快速实现项目迁移(比如我不想用阿里云的git，我就将其迁移到github上面);

本文参考资料如下:
[如何快速关联/ 修改 Git 远程仓库地址](https://blog.csdn.net/mlq8087/article/details/81360025)