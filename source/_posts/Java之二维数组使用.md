---
title: Java之二维数组使用
date: 2022-07-02 22:19:40
tags: "Java"
---


二维数组应用场景：

- 1.地图数据路径展示
- 2.特殊文件解析存储

<!--more-->

## 一、定义类
```
@Data
public class LLVO {
    private double lng;
    private double lat;
}

```

## 二、测试
```
 public static void main(String[] args) {
        //集合定义
        List<LLVO> dataList = new ArrayList<>();
        //创建对象vo1
        LLVO vo1 = new LLVO();
        vo1.setLng(1);
        vo1.setLat(2);
        //创建对象vo2
        LLVO vo2 = new LLVO();
        vo2.setLng(3);
        vo2.setLat(4);
        //创建对象vo3
        LLVO vo3 = new LLVO();
        vo3.setLng(5);
        vo3.setLat(6);
        //集合添加对应的对象数据
        dataList.add(vo1);
        dataList.add(vo2);
        dataList.add(vo3);
        //二维数组初始化
        double[][] llArr = new double[dataList.size()][];
        //循环设置数组元素
        for (int i = 0; i < dataList.size(); i++) {
            LLVO l = dataList.get(i);
            llArr[i] = new double[]{l.getLng(), l.getLat()};
        }
        //JSON打印数据(JSON打印用的是Hutool工具类，引入Hutool工具依赖即可调用)
        System.out.println("json data:" + JSONUtil.toJsonStr(llArr));
    }

```