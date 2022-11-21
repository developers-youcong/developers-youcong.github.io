---
title: Linux下word转pdf中文乱码问题
date: 2021-06-20 20:26:04
tags: "Linux"
---
最近遇到一个问题，word下载正常，word转pdf后下载出现乱码(如果是本地windos则没有问题，如果是Linux上直接显示乱码)。

最后通过搜索找到了原因:
原因之所以Windows不会有乱码在于C:\Windows\Fonts(有windows丰富的字体库，而Linux很缺乏)。

其实早在很久以前搭建WordPress站点的时候就遇到这样的乱码问题，那个时候也是将Windows的字体库上传解决的。

**解决问题步骤如下:**
<!--more-->

#### 1.上传windows字体库所有文件到Linux上
将C:\Windows\Fonts全部上传，上传的方式可以在git bash终端敲scp命令，也可以采用Xftp或WinScp等工具。

**注意:**
上传前，需要在/usr/share/fonts/建一个新的目录，名字叫winFonts(mkdir winFonts)。


#### 2.生成字体索引文件(执行两条命令)
```
sudo mkfontscale
sudo mkfontdir

```

#### 3.更新字体缓存
```
sudo fc-cache -fv

```

#### 4.重启服务器
```
reboot

```

按照上述步骤成功解决了问题。Linux环境为CentOS7.x，亲试有效。
主要参考这位朋友的文章:
[Linux下word转pdf中文乱码问题](https://blog.csdn.net/qq_40102178/article/details/100738793)


