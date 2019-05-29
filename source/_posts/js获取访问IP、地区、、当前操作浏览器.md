---
title: js获取访问IP、地区、、当前操作浏览器
date: 2019-03-21 13:18:25
tags: "javascript"
---
js获取IP、地区、当前操作浏览器有什么用呢？

我的回答是用处很多，比如现在的异地登录和对用户常用浏览器做数据分析等。

<!--more-->

源代码如下:
index.html
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Document</title>
    <script src="http://pv.sohu.com/cityjson?ie=utf-8"></script> 
    <script type="text/javascript">  
        document.write('IP地址:' + returnCitySN["cip"] + ', CID:' + returnCitySN["cid"] + ', 地区:' + returnCitySN["cname"]+",浏览器版本:"+getBrowserInfo());
		
		function getBrowserInfo()
{
    var agent = navigator.userAgent.toLowerCase() ;

    var regStr_ie = /msie [\d.]+;/gi ;
    var regStr_ff = /firefox\/[\d.]+/gi
    var regStr_chrome = /chrome\/[\d.]+/gi ;
    var regStr_saf = /safari\/[\d.]+/gi ;
    
    //IE
    if(agent.indexOf("msie") > 0)
    {
        return agent.match(regStr_ie) ;
    }

    //firefox
    if(agent.indexOf("firefox") > 0)
    {
        return agent.match(regStr_ff) ;
    }

    //Chrome
    if(agent.indexOf("chrome") > 0)
    {
        return agent.match(regStr_chrome) ;
    }

    //Safari
    if(agent.indexOf("safari") > 0 && agent.indexOf("chrome") < 0)
    {
        return agent.match(regStr_saf) ;
    }

}
    </script>
</head>

<body> 
</body>
</html>

```

展示效果如下:
![](js获取访问IP、地区、、当前操作浏览器/js01.png)

