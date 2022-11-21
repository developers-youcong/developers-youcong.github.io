---
title: Java之13位时间戳转换为常规格式时间
date: 2022-06-25 19:28:29
tags: "Java"
---

针对13位时间戳转换为常规格式的时间，核心代码如下：
<!--more-->

```
 public static String timeConvertStamp(String targetTime) {
        Long timeLong = Long.parseLong(targetTime);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//要转换的时间格式
        Date date;
        try {
            date = sdf.parse(sdf.format(timeLong));
            return sdf.format(date);
        } catch (ParseException e) {
            e.printStackTrace();
            return null;
        }
    }
```