---
title: >-
  UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-2:
  ordinal not in range(128)
date: 2020-04-05 23:33:40
tags: "Python"
---
错误背景:
使用Python2.7写一个简单爬虫报的错。

错误详细信息如下:
```
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-2: ordinal not in range(128)

```
错误原因:
1.python默认使用ASCII处理字符流。

2.Unicode编码与ASCII编码的不兼容，Python脚本文件是由utf-8编码的。


解决方法(在当前python文件最上面加上如下代码):
```
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
```
