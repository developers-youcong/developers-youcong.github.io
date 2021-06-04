---
title: Drools之关键字及错误信息
date: 2020-07-13 15:45:12
tags: "Drools"
---

## 一、关键字说明
Drools规则引擎有硬关键字和软关键字之分。
<!--more-->
硬关键字为被保留，命名相关定义时，如对象、属性、方法、函数和应用于规则文本中的其他元素，编辑规则内容时不能使用硬关键字作为命名规范。硬关键字主要包括true、false、null。

编写规则时，一定要注意软关键字不像硬关键字那么强制，软关键字相比硬关键字要多，如果非要使用软关键字作为命名是没有问题的。

软关键字有:lock-on-active、date-effective、date-expires、no-loop、auto-focus、activation-group、agenda-group、ruleflow-group、entry-point、duration、package、import、dialec、salience、enabled、attributes、rule、extend、template、query、declare、function、global、eval、not、in、or、and、exists、forall、action、reverse、result、end、init等。


DRL语言的另一个改进是可以在规则文本中转移硬关键字。这个功能可以在编辑规则内容时减少使用关键字所带来的语法错误。编写规则内容时只需将当前关键字进行转义即可，如Holiday('when'=="july")，只需"'"符号括起来就可以解决语法错误问题。

规则内容的任何地方都可以使用转义，但不包含LHS或RHS代码块中表达参数的代码。


## 二、错误信息
Drools5以后引入了一个标准化的错误信息。标准化的目的在于更快、更容易帮助用户发现和解决问题。

1st Block:指错误代码。

2nd Block:指行列。

3rd Block:描述问题。

4th Block:指发生错误的规则名、函数、模板、查询等。

5th Block:指发生错误的pattern。