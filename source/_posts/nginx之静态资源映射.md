---
title: nginx之静态资源映射
date: 2020-11-30 20:49:04
tags: "Linux"
---

**主要应用场景:**
访问nginx不同目录下的静态资源。
<!--more-->
**核心配置代码:**
```
 location /img/ {
            alias /home/software/nginx/html/img/;
            autoindex on;
 }


```

