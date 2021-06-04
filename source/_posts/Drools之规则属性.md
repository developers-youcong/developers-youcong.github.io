---
title: Drools之规则属性
date: 2020-07-13 13:59:23
tags: "Drools"
---

## 一、属性no-loop
默认值:false
类型:Boolean
属性说明:防止死循环，当规则通过update之类的函数修改了Fact对象时，可能使规则再次被激活，从而导致死循环。将no-loop设置为true的目的是避免当前规则then部分被修改后的事实对象再次被激活，从而防止死循环的发生。
<!--more-->

## 二、属性ruleflow-group
默认值:N/A
类型:String
属性说明:ruleflow-group分为rule、flow和group3个部分，分别代表规则、流程、分组，即常说的规则流。

## 三、属性lock-on-active
默认值:false
类型:Boolean
属性说明:lock-on-active是指"锁定活跃"。既然它是规则体的属性，那一定是锁定规则的，而且是锁定活跃的规则。简单地说，当规则体设置该属性为true时，则当前只会被触发一次。当ruleflow-group或agenda-group再次被激活时，即使在规则体中设置lock-on-active为true,该规则体也不能再次被激活，即无论如何更新规则事实对象，当前规则也只能被触发一次。这是no-loop的升级版，一个更强大的解决死循环的属性。

## 四、属性salience
默认值:0
类型:integer
属性说明:规则体被执行的顺序，每一个规则都有一个默认的执行顺序，如果不设置salience属性，规则体的执行顺序为由上到下。salience值可以是一个整数，但也可以是一个负数，其值越大，执行顺序越高，排名越靠前。Drools还支持动态配置优先级。

## 五、属性enabled
默认值:true
类型:Boolean
属性说明:指规则是否可以被执行，若规则体设置为enabled false,则规则体将视为永久不被激活。

## 六、属性dialect
可能值:Java或Mvel。
类型:String。
属性说明:用来定义规则中要使用的语言类型，支持Mvel和Java两种类型语言，默认情况下由包指定的。Java语言在特殊情况下会用到，如Ac-cumulate、引用Java中的语法等。

## 七、属性date-effective
默认值:N/A
类型:String、日期、时间。
属性说明:只有当前系统时间大于等于设置的时间或日期，规则才会被激活。在没有设置该属性的情况下，规则体不受时间限制。date-effective的值是一个日期型的字符串，默认情况下，date-effective可接受的日期格式为"dd-MM-yyyy"。

## 八、属性date-expires
默认值:N/A
类型:String、日期、时间。
属性说明:date-expires属性与date-effective属性是相反的，即只有当前系统时间小于设置的时间或日期，规则才会被激活。在没有设置该属性的情况下，规则体不受时间限制。date-expires的值为一个日期型的字符串，默认情况下，date-expires可接受的日期格式为"dd-MMM-yyyy"。

## 九、属性duration
默认值:无。
类型:long。
属性说明:表示定时器，如果当前规则LHS部分为true，那么规则继续执行;如果该属性已经被弃用，那么通过新的属性timer来控制。

## 十、属性activation-group
默认值:N/A。
类型:String。
属性说明:activation-group是指激活分组，通过字符串定义分组名称，具有相同组名称的规则体有且只有一个规则被激活，其他规则体的LHS部分仍然为true也不会被执行。该属性受salience属性影响，如当前规则文件中的其他规则未设计该属性，则视为规则处于被激活状态，并不受该属性的影响。

## 十一、属性agenda-group
默认值:无，需要通过Java设置。
类型:String。
属性说明:agenda-group是议程分组，属于另一种可控的规则执行方式，是指用户可以通过配置agenda-group的参数来控制规则的执行，而且只有获取焦点的规则才会被激活。

## 十二、属性auto-focus
默认值:false。
类型:Boolean。
属性说明:auto-focus属性为自动获取焦点，即当前规则是否被激活。如果一个规则被执行，那么认为auto-focus为true;如果单独设置，一般结合agenda-group，当一个议程组未获取焦点时，可以设置auto-focus来控制。

## 十三、属性timer
默认值:无。
类型:与Java定时器参数类型相似。
属性说明:timer属性是一个定时器，用来控制规则的执行时间。