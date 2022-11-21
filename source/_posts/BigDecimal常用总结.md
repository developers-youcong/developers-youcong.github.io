---
title: BigDecimal常用总结
date: 2021-11-30 19:54:28
tags: "Java"
---
近来数据运算方面用BigDecimal比较多，做一个常用的总结归纳。
<!--more-->

## 一、BigDecimal加减乘除
```
        BigDecimal num1 = new BigDecimal(5);
        BigDecimal num2 = new BigDecimal(10);
        BigDecimal num3 = new BigDecimal(15);

        BigDecimal num4 = new BigDecimal("20");
        BigDecimal num5 = new BigDecimal("25");
        BigDecimal num6 = new BigDecimal("30");

		//加法
        BigDecimal result1 = num1.add(num2);
        BigDecimal result2 = num4.add(num6);

        //减法
        BigDecimal result3 = num1.subtract(num2);
        BigDecimal result4 = num5.subtract(num4);

        //乘法
        BigDecimal result5 = num1.multiply(num2);
        BigDecimal result6 = num6.multiply(num5);

        //除法
        BigDecimal result7 = num2.divide(num1, 2, BigDecimal.ROUND_HALF_UP);
        BigDecimal result8 = num6.divide(num1, 2, BigDecimal.ROUND_HALF_UP);
		//结果输出
        System.out.println("result1:" + result1);
        System.out.println("result2:" + result2);
        System.out.println("result3:" + result3);
        System.out.println("result4:" + result4);
        System.out.println("result5:" + result5);
        System.out.println("result6:" + result6);
        System.out.println("result7:" + result7);
        System.out.println("result8:" + result8);

```

## 二、BigDecimal平均数、总和、最大值、最小值
```
        //求最大值
        BigDecimal max = dataList.stream().map(T::getPrice).max((x1, x2) -> x1.compareTo(x2)).get();
        //求最小值
        BigDecimal min = dataList.stream().map(T::getPrice).min((x1, x2) -> x1.compareTo(x2)).get();
        //求和 空指针异常排除
        BigDecimal sum = dataList.stream().map(vo -> ObjectUtils.isEmpty(vo.getPrice()) ? new BigDecimal(0) : vo.getPrice()).reduce(BigDecimal.ZERO, BigDecimal::add);
        //求平均值
        BigDecimal average = dataList.stream().map(vo -> ObjectUtils.isEmpty(vo.getPrice()) ? new BigDecimal(0) : vo.getPrice()).reduce(BigDecimal.ZERO, BigDecimal::add).divide(BigDecimal.valueOf(dataList.size()), 2, BigDecimal.ROUND_HALF_UP);



```
## 三、BigDecimal大小比较
```
		//大小比较
        if(num2.compareTo(new BigDecimal(10)) == 0){
            System.out.println("等于");
        }

        if(num4.compareTo(new BigDecimal(10))>0){
            System.out.println("大于");
        }

        if(num1.compareTo(new BigDecimal(10))<0){
            System.out.println("小于");
        }


```

## 四、BigDecimal常见问题
- 相除精度丢失问题(将需要计算的数值转换为字符串并注入到BigDecimal中就能解决)；
- 舍入精度丢失问题(参考后面的八种舍位模式)。

## 五、BigDecimal常用方法
```
BigDecimal add(BigDecimal value);
BigDecimal subtract(BigDecimal value);
BigDecimal multiply(BigDecimal value);
BigDecimal divide(BigDecimal value);
BigDecimal toString(BigDecimal value);
BigDecimal valueOf(BigDecimal value);
BigDecimal compareTo(BigDecimal value);
BigDecimal remainder(BigDecimal value);
BigDecimal abs(BigDecimal value);
BigDecimal max(BigDecimal value);
BigDecimal min(BigDecimal value);
BigDecimal negate(BigDecimal value);
BigDecimal remainder(BigDecimal value)：
```

## 六、BigDecimal如何格式化
```
        DecimalFormat df1 = new DecimalFormat("0.0");
        DecimalFormat df2 = new DecimalFormat("#.#");
        DecimalFormat df3 = new DecimalFormat("000.000");
        DecimalFormat df4 = new DecimalFormat("###.###");
        System.out.println(df1.format(9.28));
        System.out.println(df2.format(10.28));
        System.out.println(df3.format(11.28));
        System.out.println(df4.format(12.28));

```

## 七、BigDecimal八种舍位模式

### 1.ROUND_UP
舍入远离零的舍入模式。
在丢弃非零部分之前始终增加数字(始终对非零舍弃部分前面的数字加1)。
**注意:**此舍入模式始终不会减少计算值的大小。

### 2.ROUND_DOWN
接近零的舍入模式。在丢弃某部分之前始终不增加数字(从不对舍弃部分前面的数字加1，即截短)。
**注意:**此舍入模式始终不会增加计算值的大小。

### 3.ROUND_CEILING
接近正无穷大的舍入模式。
如果 BigDecimal 为正，则舍入行为与 ROUND_UP 相同;
如果为负，则舍入行为与 ROUND_DOWN 相同。
**注意:**此舍入模式始终不会减少计算值。

### 4..ROUND_FLOOR
接近负无穷大的舍入模式。如果 BigDecimal 为正，则舍入行为与 ROUND_DOWN 相同;如果为负，则舍入行为与 ROUND_UP 相同。
**注意:**此舍入模式始终不会增加计算值。

### 5.ROUND_HALF_UP
向“最接近的”数字舍入，如果与两个相邻数字的距离相等，则为向上舍入的舍入模式。如果舍弃部分 >= 0.5，则舍入行为与 ROUND_UP 相同;否则舍入行为与 ROUND_DOWN 相同。
**注意:**这是我们大多数人在小学时就学过的舍入模式(四舍五入)。

### 6.ROUND_HALF_DOWN
向“最接近的”数字舍入，如果与两个相邻数字的距离相等，则为上舍入的舍入模式。如果舍弃部分 > 0.5，则舍入行为与 ROUND_UP 相同;否则舍入行为与 ROUND_DOWN 相同(五舍六入)。

### 7.ROUND_HALF_EVEN
向“最接近的”数字舍入，如果与两个相邻数字的距离相等，则向相邻的偶数舍入。如果舍弃部分左边的数字为奇数，则舍入行为与 ROUND_HALF_UP 相同;如果为偶数，则舍入行为与 ROUND_HALF_DOWN 相同。
**注意:**在重复进行一系列计算时，此舍入模式可以将累加错误减到最小。此舍入模式也称为“银行家舍入法”，主要在美国使用。四舍六入，五分两种情况。
如果前一位为奇数，则入位，否则舍去。以下例子为保留小数点1位，那么这种舍入方式下的结果。
1.15>1.2 1.25>1.2

### 8.ROUND_UNNECESSARY
断言请求的操作具有精确的结果，因此不需要舍入。如果对获得精确结果的操作指定此舍入模式，则抛出ArithmeticException。

## 八、BigDecimal常用构造函数

### 1.创建一个具有参数所指定整数值的对象
```
BigDecimal(int)
```

### 2.创建一个具有参数所指定双精度值的对象
```
BigDecimal(double)
```

### 3.创建一个具有参数所指定长整数值的对象
```
BigDecimal(long)

```
### 4.创建一个具有参数所指定以字符串表示的数值的对象
```
BigDecimal(String)

```