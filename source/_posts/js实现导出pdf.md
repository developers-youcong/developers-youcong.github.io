---
title: js实现导出pdf
date: 2020-07-08 22:40:15
tags: "javascript"
---

## 一、下载js库
下载地址:
https://github.com/eKoopmans/html2pdf.js

官方文档:
https://ekoopmans.github.io/html2pdf.js/
<!--more-->
## 二、引入
```
<script src="../js/html2pdf.js"></script>

```
## 三、编写核心js导出pdf代码
```
        $("#download_pdf").click(function () {

            //可以是$("#id或类选择器").html()或val()
            var element = testEditor.getHTML();

            html2pdf().from(element).set({
                margin: 1,
                filename: 'resume.pdf',
                html2canvas: {scale: 2},
                jsPDF: {orientation: 'portrait', unit: 'in', format: 'letter', compressPDF: false}
            }).save();



        });

```