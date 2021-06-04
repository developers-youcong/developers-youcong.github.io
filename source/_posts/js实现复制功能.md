---
title: js实现复制功能
date: 2020-07-11 10:28:22
tags: "javascript"
---
应用场景:
最近我做的一个在线工具网站(参考现在的JSON网站做的)，其中有一个功能叫做JSON格式化和校验，通过复制JSON数据点击格式化后，得到美化的JSON数据，再点击按钮"复制"就能获取美化后JSON数据。
<!--more-->

核心代码:
```
function selectText(element) {
    var text = document.getElementById(element);

    if (document.body.createTextRange) {
        //createTextRange是用在IE中的
        var range = document.body.createTextRange();
        range.moveToElementText(text);
        range.select();
    } else if (window.getSelection) {
        var selection = window.getSelection();
        var range = document.createRange();
        range.selectNodeContents(text);
        selection.removeAllRanges();
        selection.addRange(range);
        document.execCommand("Copy")
    } else {
        alert("none");
    }
}

```