---
title: Integer不能转String问题
date: 2022-02-25 22:04:24
tags: "Java"
---
今天一同事遇到了一个问题。

关键错误信息如下:
```
java.lang.Integer cannot be cast to java.lang.String

```
<!--more-->

问题代码类似这样的:
```
ArrayList<HashMap<String, String>>


```

遍历上述的问题代码，将对应值以Map的形式获取然后存储进行处理。
然后在用toString()方法处理对应的代码过程中就报了上述的错误，这使得我很疑惑。最终用String.value()方法处理则就没问题了。
然后使我对toString()方法与String.value()方法产生了疑问。

### toString()方法与String.value()方法到底有何不同呢？

首先这两个方法的作用是将非字符串转成字符串。

**Object的toString()方法源码:**
```
    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

```


**String.value()方法源码:**
```
    public static String valueOf(Object obj) {
        return (obj == null) ? "null" : obj.toString();
    }

```

**从应用层面分析:**

(1)使用Object.toString()方法要特别注意一点，那就是必须确保Object不能为空，否则会报NullPointerException异常。

(2)使用String.value()方法则可以为空(不用担心空指针异常的影响)，从源码就能看出来。


**注意一点:**
基本数据类型没有toString()方法，只有对应的包装类才有。实际应用中必须明确，减少不必要的错误。
