---
title: Nginx代理下载文件出现失败问题之解决
date: 2021-07-15 21:57:19
tags: "Linux"
---
今日发现某个代理出现下载文件失败问题，相当于点击下载，直接提示下载失败，需要点击继续才能成功，基于这个问题我搜索查找了一下，最后发现是代理配置的有问题，这是原来的问题配置：
```
 location /file {
           proxy_pass http://127.0.0.1:8080/files;
        }
```
<!--more-->

解决问题的配置如下:
```
 location /file {
           proxy_pass http://127.0.0.1:8080/report;
           proxy_redirect default;
           proxy_buffering off;
           proxy_send_timeout 90;
           proxy_read_timeout 90;
        }
```
