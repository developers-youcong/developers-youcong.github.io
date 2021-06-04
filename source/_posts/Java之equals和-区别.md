---
title: Java之equals和==区别
date: 2020-06-14 14:14:01
tags: "Java"
---

equals和==是Java中用到频率很高的，虽然不少开发者使用第三方的JAR包如hutool中的StrUtil.isEmptyIfStr(Object obj)这个方法中源代码就是==,
如下源代码:
<!--more-->
```
public static boolean isEmptyIfStr(Object obj) {
		if (null == obj) {
			return true;
		} else if (obj instanceof CharSequence) {
			return 0 == ((CharSequence) obj).length();
		}
		return false;
	}

```

再比如我再这篇文章[SpringCloud之Security](https://developers-youcong.github.io/2020/06/10/SpringCloud%E4%B9%8BSecurity/)中的自定义登录验证就用到equals和==。

可以说不论过去还是现在，我所开发的Java系统都涉及到这两个(凡是牵涉到判断的地方基本都这么用，不排除不少公司用第三方API封装好的，但本质上都是这些的封装和判断)。

虽然在用但对其也不是非常了解，所以有必要深入。

## 1.先说说equals和==的区别
(1)对于==
如果作用于基本数据类型的变量，则直接比较其存储的 "值"是否相等;如果作用于引用类型的变量，则比较的是所指向的对象的地址。

(2)对于equals方法，
注意：equals方法不能作用于基本数据类型的变量,如果没有对equals方法进行重写，则比较的是引用类型的变量所指向的对象的地址，如果对equals方法进行重新，则比较的就是值是否相等。

## 2.以String源代码equals为例
```
    public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }

```
这段代码可以这么理解:
**在String类中equals方法不仅可以用==判断对象的内存地址是否相等，相等则返回true。如果前面的判断不成立，接着判断括号内的对象上是否是String类型，接着判断两个字符串对象的的长度是否相等，最后判断内容是否相等，如果相等则返回true。**

来个例子，如下:
```
        String s1 = "blog";

        String s2 = new String("blog");

        System.out.println(s1 == s2);//输出结果为false

        System.out.println(s1.equals(s2)); //输出结果为true

```



