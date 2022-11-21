---
title: Java判断字符串是否包含的三种方法
date: 2022-10-30 17:41:21
tags: "Java"
---

基于startsWith、contains、indexOf等三种方法封装。
<!--more-->

## 一、基于startsWith方法
```
/**
 * 字符串前缀是否匹配
 *
 * @param prefix
 * @param targetStr
 * @return
 */
public static boolean isStarsWith(String prefix, String targetStr) {
    if (prefix != null && prefix != "" && targetStr != null && targetStr != "") {
        return targetStr.startsWith(prefix);
    }
    return false;
}

```

## 二、基于contains方法
```
/**
 * 字符串是否包含(基于contains方法)
 *
 * @param targetStr
 * @param contains
 * @return
 */
public static boolean isContains(String targetStr, String contains) {
    if (targetStr != null && targetStr != "" && contains != null && contains != "") {
        return targetStr.contains(contains);
    }
    return false;
}

```

## 三、基于indexOf方法
```
/**
 * 字符串是否包含(基于indexOf方法)
 *
 * @param targetStr
 * @param indexOfStr
 * @return
 */
public static int isIndexOf(String targetStr, String indexOfStr) {
    if (targetStr != null && targetStr != "" && indexOfStr != null && indexOfStr != "") {
        int result = targetStr.indexOf(indexOfStr);
        if (result != -1) {
            return result;
        } else {
            return -1;
        }
    }
    return -1;
}

```