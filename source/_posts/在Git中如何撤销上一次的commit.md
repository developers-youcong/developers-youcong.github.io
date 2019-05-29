---
title: 在Git中如何撤销上一次的commit
date: 2019-05-15 11:58:39
tags: "Git"
---
有的时候我们一不小心就git commit -m 'commit message info'
解决办法，很简单，只需执行`git reset HEAD~ `这条命令即可，即能保证你原本的修改还在，也能撤销本次提交失误。

这种撤销上一次提交是比较好的，如果是版本回退的话也能解决这个问题，但是版本回退只适合于你本次提交并没有改动什么或者改动不大的情况。如果你改动太多，版本回退意味着着你需要重新复制一遍，当然了，解决这种问题的办法有很多，分支开发的方式也能解决这种问题。
<!--more-->
顺便补充到，如果git add 失误呢？如何解决呢？
执行如下命令即可:
```
git rm -r dir_name --cached

```
dir_nameo可以是.也可以是你git add 某个目录，如git add src/
你只需git rm -r src/ --cached 便可删除git add src/ 添加到的暂存区，从而达到撤销git add 的失误操作


[git撤销add操作](https://blog.csdn.net/svap1/article/details/80537198)
[[译] 在Git中如何撤销上一次的commit(s)？](https://www.jianshu.com/p/9f11d398111f)
