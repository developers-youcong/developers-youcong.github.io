---
title: >-
  SyntaxError: Non-ASCII character 'æ' in file csdn.py on line 7, but no
  encoding declared; see http://python.org/dev/peps/pep-0263/ for details
date: 2019-05-03 22:10:12
tags: "Python"
---

错误信息:
```
SyntaxError: Non-ASCII character 'æ' in file csdn.py on line 7, but no
encoding declared; see http://python.org/dev/peps/pep-0263/ for details

```


错误原因:
原因是因为Python在默认状态下不支持源代码中的编码所致。
<!--more-->
解决方案:
在Python文件开头加上`# -*- coding: utf-8 -*`即可解决该问题

