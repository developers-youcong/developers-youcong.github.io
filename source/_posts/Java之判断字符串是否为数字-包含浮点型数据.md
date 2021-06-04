---
title: Java之判断字符串是否为数字(包含浮点型数据)
date: 2021-02-18 21:40:47
tags: "Java"
---

核心代码如下(利用正则表达式判断):
```
    public static boolean isNumber(String str) {

        if (StringUtils.isNotEmpty(str)) {
            String reg = "^[0-9]+(.[0-9]+)?$";
            return str.matches(reg);
        }
        return false;
    }

```
<!--more-->
如果仅仅是非浮点型数据，也可以使用下面这段代码:
```
    public static boolean isNumberJava(String str) {
        for (int i = 0; i < str.length(); i++) {
            if (!Character.isDigit(str.charAt(i))) {
                return false;
            }
        }
        return true;
    }

```

不管浮点或者非浮点类型数据，我个人倾向于正则表达式，因为它看起来非常简洁。