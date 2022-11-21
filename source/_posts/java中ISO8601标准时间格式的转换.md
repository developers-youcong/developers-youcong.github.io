---
title: java中ISO8601标准时间格式的转换
date: 2022-04-25 21:54:45
tags: "Java"
---

某些场景我们需要将常规的时间转换为ISO8601标准时间格式，核心代码如下:
<!--more-->
```
public static String convertISO8601TimestampFromDateStr(String timestamp) {
        java.time.format.DateTimeFormatter dtf1 = java.time.format.DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        LocalDateTime ldt = LocalDateTime.parse(timestamp, dtf1);
        ZoneOffset offset = ZoneOffset.of("+08:00");
        OffsetDateTime date = OffsetDateTime.of(ldt, offset);
        java.time.format.DateTimeFormatter dtf2 = java.time.format.DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss'Z'");
        return date.format(dtf2);
    }

    public static void main(String[] args) {
        System.out.println("date start1:" + getISO8601TimestampFromDateStr("2022-04-25 21:56:00"));
    }

```

参考资料如下:
[java中ISO8601标准时间格式的转换](https://blog.csdn.net/snowwithrain/article/details/108829259
)