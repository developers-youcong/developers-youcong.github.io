---
title: js实现美化json数据格式
date: 2020-07-11 10:32:35
tags: "javascript"
---

## 一、核心方法代码
<!--more-->
```
//json格式美化
function prettyFormat(str) {
    try {
        // 设置缩进为2个空格
        str = JSON.stringify(JSON.parse(str), null, 2);
        str = str
            .replace(/&/g, '&amp;')
            .replace(/</g, '&lt;')
            .replace(/>/g, '&gt;');
        return str.replace(/("(\\u[a-zA-Z0-9]{4}|\\[^u]|[^\\"])*"(\s*:)?|\b(true|false|null)\b|-?\d+(?:\.\d*)?(?:[eE][+\-]?\d+)?)/g, function (match) {
            var cls = 'number';
            if (/^"/.test(match)) {
                if (/:$/.test(match)) {
                    cls = 'key';
                } else {
                    cls = 'string';
                }
            } else if (/true|false/.test(match)) {
                cls = 'boolean';
            } else if (/null/.test(match)) {
                cls = 'null';
            }
            return '<span class="' + cls + '">' + match + '</span>';
        });
    } catch (e) {
        alert("异常信息:" + e);
    }


}

```

## 二、函数调用
```
 $("#show").html("<pre>" + prettyFormat(data) + "</pre>");


```

## 三、html中引用
```
<div id="show"></div>

```