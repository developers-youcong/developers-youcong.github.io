---
title: Java之for循环内外创建对象的区别
date: 2021-03-21 21:11:15
tags: "Java"
---
## for循环内外创建对象的区别，哪个性能更优？
<!--more-->

for循环内创建对象，就像这样的代码:
```
        List<DriverTrack> driverTracks = driverService.selectDriverTrackByExample(example);
        List<TrackVo> list = new ArrayList<TrackVo>();
        if (driverTracks != null && driverTracks.size() > 0) {
            for (DriverTrack driverTrack : driverTracks) {
                TrackVo trackVo = new TrackVo();
                trackVo.setLat(driverTrack.getLatitude());
                trackVo.setLng(driverTrack.getLongitude());
                list.add(trackVo);
            }
        }

```

for循环外创建对象，就像这样的代码:
```
        List<DriverTrack> driverTracks = driverService.selectDriverTrackByExample(example);
        List<TrackVo> list = new ArrayList<TrackVo>();
        if (driverTracks != null && driverTracks.size() > 0) {
            TrackVo trackVo = null;
            for (DriverTrack driverTrack : driverTracks) {
                trackVo = new TrackVo();
                trackVo.setLat(driverTrack.getLatitude());
                trackVo.setLng(driverTrack.getLongitude());
                list.add(trackVo);
 }
        }


```

**两者写法的对比存在争议,有如下观点:**
- A认为后者比前者要好(因为这样写只创建了一个对象的引用，也就是在for循环里面去new对象的时候，都只是将这个引用指向不同的对象)；
- B认为随着JDK不断升级迭代，两者效率是一样；
- C认为JVM早就解决这样的问题，无需担心。

**我的看法:**
我更偏向于for循环体内定义对象，因为我一直的写法也是如此，特别是JDK从过去的1.5到现在，JDK已经有15了，像这样的问题JDK开发者早已经替我们考虑好了(Java不像C++,对内存的把控非常严，因为已经有JVM替我们管理了，我们只需专注于业务)。
但是从另外一个角度来看，弄清楚为什么比仅仅停留在使用层面，能让我们对于这项技术有更深入的了解和掌握，深入的了解和掌握能让我们走得更远。最近公司领导就特别建议在循环体外定义对象而非在循环体内定义对象。

接着留一个问题，供我以及感兴趣的读者实践研究？
for循环体外性能对比，拿出实际的数据证明两者的优劣。

参考资料如下:
[【JAVA】变量声明在循环体内还是循环体外的争论，以及怎样才真正叫『避免在循环体中创建对象』？](https://www.zhihu.com/question/31751468)

[java中的for循环里面创建对象和for循环外面创建对象之间的区别](https://blog.csdn.net/superman__007/article/details/73549921)

[for循环中创建对象](https://www.jianshu.com/p/744587b61148)