---
title: user.name has multiple values
date: 2020-03-30 22:40:42
tags: "Git"
---

详细错误信息如下:
```
warning: user.email has multiple values
error: cannot overwrite multiple values with a single value
       Use a regexp, --add or --replace-all to change user.email.


```

**错误原因:**
通过git config --list命令 发现有多个user.name
<!--more-->

**错误导致的结果:**
直接导致git commit 后贡献度不增加，因为不识别用户提交。

**解决问题办法(执行如下代码即可):**
```
git config --global --replace-all user.name "输入你的用户名"
git config --global --replace-all user.email "输入你的邮箱" 

```

最后再执行git config --list，发现不再出现重复user.name表示成功。

