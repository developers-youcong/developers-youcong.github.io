---
title: Drools之基础语法
date: 2020-07-04 13:48:19
tags: "Drools"
---

## 一、规则文件
标准的规则文件以".drl"结尾。
<!--more-->
一套完整的规则文件内容如下:
- package:包名，只限于逻辑上的管理，若自定义的查询或函数位于同一包名，不管物理位置如何，都可以直接调用。
- import:规则引用问题，导入类或静态方法。
- global:全局变量，使用时需要单独定义变量类型
- function:自定义函数，可以理解为Java静态方法的一种变形，与JavaScript函数定义相似。
- queried:查询。
- rule end:规则内容中的规则体，是进行业务规则判断、处理业务结果的部分。


## 二、规则体语法结构
一个规则体包含三个部分，唯有attributes部分是可选，其他关键字都是必填信息。属性可选并不表示没有，属性是有默认值的，如规则默认是被激活的。
规则体语法结构如下:
- rule:规则开始，参数是规则的唯一名称
- attributes:规则属性，是rule与when之间的参数，为可选项
- when:规则条件部分，默认为true
- then:规则结果部分
- end:当前规则结束

## 三、匹配模式
LHS部分由一个或多个条件组成，条件又称pattern。多个pattern之间既可以使用and或or连接，又可以使用小括号确定pattern的优先级，默认条件是true。

pattern的"绑定变量名"是可选的，当前规则的RHS部分需要操作pattern匹配的参数，若要用到某些对象，则可以通过为该对象设定一个绑定变量名来实现对它的操作。对于绑定变量的命名，通常是为其添加一个"$"符号作为前缀，与对象的命名方法相同;绑定变量不仅可以用在对象上，也可以用在对象的属性上，作用是方便RHS部分的操作，同时也避免与Fact对象属性的使用相混淆。

"Field约束"是指当前对象中属性或方法的使用，如添加条件限制"name=='YC',age==24".

规则体中LHS部分绑定变量基本上有两种形式:一种是整个Fact变量的绑定，另一种是约束条件属性变量的绑定。

## 四、运算符
运算符是在程序中最常用的计算方法，一般的运算符包括"+、-、*、/、%"等，优先级与Java相同。

## 五、约束连接
匹配模式中有多种约束符的连接，常用的有"&&”(and)、"||"(or)、","(and)。这3个连接符号如果没有用括号来显示定义的优先级，那么"&&"优先级大于"||"优先级。

Drools自带的约束，共有6种比较操作符。

### 1.contains比较操作符
contains是用来检查一个Fact对象的某个属性值是否包含一个指定的对象值。其语法格式为:
```
Object[field[Collection/Array] contains | not contains value]

```

### 2.not contains 比较运算符
not contains的作用与contains相反，它是用来判断一个Fact对象的某个字段不包含一个指定的对象。

### 3.memberOf比较运算符
memberOf用来判断某个Fact对象的某个字段是否在一个或多个集合中。其语法格式为:
```
Object(fieldName memberOf | not memberOf value[Collection/Array])

```

### 4.not memberOf比较运算符
not memberOf与memberOf的作用相反，是用来判断Fact对象中某个字段不在某个集合中。

### 5.matches比较运算符
matches用来对某个Fact对象的字段与标准的Java正则表达式进行相似匹配，被比较的字符串可以是一个标准的Java正则表达式。但需要注意的是，正则表达式字符串中不用考虑"\"的转义问题，其语法为:
```
Object(fieldName matches | not matches "正则表达式")

```

### 6.not matches 比较运算符
not matches的作用与matches相反，是用来将某个Fact对象的字段与一个Java标准正则表达式进行匹配，若与正则表达式不匹配，则规则成立。

### 7.soundlike比较运算符
soundlike用来检查单词是否具有与给定值几乎相同的声音(使用英语发音)。基于Soundex算法的语法为:
```
Object(fieldName soundlike 'value')

```

### 8.str比较运算符
str不仅检查String字段是否以某一值开头/结尾，还可以判断字符串长度，其语法为:
```
Object(fieldName str[startWith|endWith|length] "String"|1)

```

## 六、语法扩展
主要指List、Set、Map等元素操作。

## 七、规则文件drl

### 1.单行注释(使用"//"进行标记)

### 2.多行注释(以"`/*`"开始，以"`*/`"结束)