---
title: Git删除某一次提交
date: 2022-04-04 15:26:33
tags: "Git"
---

在日常开发中，采用Git作为版本控制的过程中，我们总会有这样的需求，当前提交的本地记录有问题，我想将其丢弃。针对这样的需求，Git有三种解决办法。
<!--more-->

## 一、git reset
```
git reset      #回滚到某次提交。
git reset soft #此次提交之后的修改会被退回到暂存区
git reset hard #此次提交之后的修改不做任何保留，git status 查看工作区是没有记录的。

```

回滚代码，例如:
```
步骤一:git log  #查询要回滚的 commit_id
步骤二:git reset --hard commit_id  #HEAD 就会指向此次的提交记录
步骤三:git push origin HEAD --force #强制推送到远端

```

误删恢复，例如:
```
步骤一:git relog  #复制要恢复操作的前面的 hash 值
步骤二:git reset --hard hash #将 hash 换成要恢复的历史记录的 hash 值

```

## 二、git rebase
git rebase：当两个分支不在一条线上，需要执行 merge 操作时使用该命令。

撤销提交，例如:
```
步骤一:git log # 查找要删除的前一次提交的 commit_id
步骤二:git rebase -i commit_id # 将 commit_id 替换成复制的值
步骤三:进入 Vim 编辑模式，将要删除的 commit 前面的 `pick` 改成 `drop`
步骤四:保存并退出 Vim

```

解决冲突，例如:
```
步骤一:git diff # 查看冲突内容
步骤二:手动解决冲突（冲突位置已在文件中标明）
步骤三:git add <file> 或 git add -A # 添加
步骤四:git rebase --continue # 继续 rebase
步骤五:若还在 rebase 状态，则重复 2、3、4，直至 rebase 完成出现 applying 字样
步骤六: git push

```

## 三、git revert
```
git revert #放弃某次提交。
git revert #之前的提交仍会保留在 git log 中，而此次撤销会做为一次新的提交。
git revert -m #用于对 merge 节点的操作，-m 指定具体某个提交点。

```

撤销提交，例如:
```
步骤一:git log #查找需要撤销的 commit_id
步骤二:git revert commit_id   #撤销这次提交
```

撤销merge节点提交，例如:
```
步骤一: git revert commit_id -m 1 #第一个提交点
步骤二:手动解决冲突
步骤三:git add -A
步骤四:git commit -m ""
步骤五:git revert commit_id -m 2 #第二个提交点
步骤六: 重复 二，三，四
步骤七:git push
```

本文主要参考资料如下:
[Git 删除某一次提交](https://www.jianshu.com/p/0eb17bc695ed)