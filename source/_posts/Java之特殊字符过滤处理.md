---
title: Java之特殊字符过滤处理
date: 2021-05-16 12:18:19
tags: "Java"
---

核心方法如下(结合正则表达式和Matcher的方法进行替换):
<!--more-->
```
    public static String strSpecialFilter(String str) {
        String regEx = "[\\u00A0\\s\"`~!@#$%^&*()+=|{}':;',\\[\\].<>/?~！@#￥%……&*（）——+|{}【】‘；：”“’。，、？]";
        Pattern p = Pattern.compile(regEx);
        Matcher m = p.matcher(str);
        //将所有的特殊字符替换为空字符串
        return m.replaceAll("").trim();
    }

```