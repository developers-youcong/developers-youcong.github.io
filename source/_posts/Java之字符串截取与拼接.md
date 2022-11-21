---
title: Java之字符串截取与拼接
date: 2022-10-30 17:49:51
tags: "Java"
---

## 一、字符串截取
<!--more-->

```
/**
 * 字符串截取并转为List
 *
 * @param targetStr
 * @param splitRule
 * @return
 */
public static List<String> strSplitHandle(String targetStr, String splitRule) {
	List<String> handeStrList = new ArrayList<>();
	if (targetStr != null && targetStr != "" && splitRule != null && splitRule != "") {
		String arr[] = targetStr.toString().split(splitRule);
		if (arr.length > 0) {
			handeStrList = Stream.of(arr).collect(Collectors.toList());
		}
	}
	return handeStrList;
}


```

这里解决的问题是：将前端字符串传参截取并转为后端List，应用于后端数据访问层对应的代码处理(例如mybatis的in传参之类的)。

## 二、字符串拼接
```
/**
 * 字符串拼接
 *
 * @param strList
 * @param appendFlag
 * @return
 */
public static String handleBuilderAppend(List<String> strList, String appendFlag) {
	StringBuilder strBuilder = new StringBuilder();
	if (!strList.isEmpty()) {
		for (String str : strList) {
			strBuilder.append(str + appendFlag);
		}
	}
	return String.valueOf(strBuilder.substring(0, strBuilder.length() - 1));
}

```

这里解决的问题是：将后端对应的List数据转为特定字符串(一般以某种特殊符号进行拼接而成，例如以逗号分割之类的等)并返回给前端展示。