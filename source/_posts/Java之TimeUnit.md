---
title: Java之TimeUnit
date: 2022-03-09 20:05:55
tags: "Java"
---

TimeUnit是Java.util.concurrent包下面的一个类。

它的主要作用有两个方面:

- 时间颗粒度转换；
- 延时。
<!--more-->

## 一、时间颗粒度转换

对应的API如下:
```
    public long toMillis(long d)    //转化成毫秒
    public long toSeconds(long d)  //转化成秒
    public long toMinutes(long d)  //转化成分钟
    public long toHours(long d)    //转化成小时
    public long toDays(long d)     //转化天

```

例子:
```
public class TimeUnitTest {
    public static void main(String[] args) {
        //1天有24个小时    1代表1天：将1天转化为小时
        System.out.println( TimeUnit.DAYS.toHours( 1 ) );
        //结果： 24

        //1小时有3600秒
        System.out.println( TimeUnit.HOURS.toSeconds( 1 ));
        //结果3600

        //把3天转化成小时
        System.out.println( TimeUnit.HOURS.convert( 3 , TimeUnit.DAYS ) );
        //结果是：72
    }
}

```

## 二、延时
延时的作用与Thread.sleep()作用是一样的，例子如下:
```
public class TimeUnitTest {
    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    TimeUnit.SECONDS.sleep(10);
                    System.out.println("延时10秒");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}

```