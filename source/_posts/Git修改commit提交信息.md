---
title: Git修改commit提交信息
date: 2020-04-13 22:58:18
tags: "Git"
---

有些时候不小心git commit -m '提交信息'中的提交信息写错了。
<!--more-->
不怕，执行如下命令即可修改(注意，仅仅只能针对最后一次提交):
```
git commit --amend -m "新的修改提交信息"

```

参考解决问题地址:
https://www.softwhy.com/article-8492-1.html