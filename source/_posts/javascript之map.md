---
title: javascript之Map
date: 2019-03-07 20:28:41
tags: "javascript"
---

javascript中的map，我用的不是特别多，倒是Java中的Map或HashMap，经常用。

顺便围绕几个方面介绍一下map？

## 1.Map对象

Map对象是一种有对应键值对的对象，JS的Object也是键值对的对象。

ES6中的Map相对Object对象有几个区别?
(1)Object对象有原型，也就是说它有默认的key值在对象上面，除非我们使用Object.create(null)创建一个没有原型的对象;
(2)在Object中，只能把String和Symbol作为key值，但是在Map中，key值可以是任何基本类型(String,Number,Boolean,undefined,NaN...),或者对象(Map,Set,Object,Function,Symbol,null...);
(3)通过Map中的size属性，可以很方便地获取Map长度，要获取Object的长度，你只能用别的方法;

Map实例对象的key值可以为一个数组或者一个对象，或者一个函数，比较随意，而且Map对象实例中数据的排序是根据用户push的顺序进行排序的，而Object实例中key,value的顺序则有些规律(它们会先排数字开头的值，然后才是字符串开头的key值);


## 2.Map实例属性
map.size这个属性和数组的length功能一样，都表示当前实例的长度。

## 3.Map实例的方法

clear() 删除所有的键值对;
delete(key) 删除指定键;
entries() 返回一个迭代器，迭代器按照对象的插入顺序返回[key,value];
forEach(callback,context) 循环执行函数并把键值对作为参数，context为执行函数的上下文this;
get(key) 返回Map对象key相对的value值;
has(key) 返回布尔值，判断Map对象是否存在指定的key;
keys() 返回一个迭代器，迭代器按照插入的顺序返回每一个key元素;
set(key,value) 给Map对象设置key/value键值对，返回这个Map对象(相对于JavaScript的Set，Set对象添加元素的方法叫add,而Map对象添加元素的方法为set)
iterator 和entireds()方法一样，返回一个迭代器，迭代器按照对象的插入顺序返回[key,value]

<!--more-->

代码示例如下(这段与后台交互的代码，主要是为了测试):
```
<html>
<head>
<script src="jquery-1.8.0.min.js"></script>
</head>
<body>

<script>

$.ajax({
    url:"http://localhost:2019/comments/recentsComments",
    type:"GET",
    contentType: 'application/json;charset=utf-8',
    dataType : 'json',
    success:function(data){
 
    console.log(data.code);

    var m = new Map();

    m.set("data",data.list);

    console.log(m.get("data"));

    },error:function(){

    }
});




</script>
</body>
</html>

```

本文主要参考链接如下所示:
ES6新特性:JavaScript中的Map和WeakMap对象:https://www.cnblogs.com/diligenceday/p/5484130.html