---
title: Linux之利用mail和sendmail发送邮件
date: 2021-06-20 22:34:55
tags: "Linux"
---
对于Linux，邮件发送的主要应用场景为告警，一旦某个服务或软件挂掉，通过邮件的形式通知相关人员(运维或其它)，让其第一时间迅速解决该问题。

<!--more-->

## 一、安装mailx和sendmail
```
yum install -y mailx sendmail

```

## 二、修改配置文件(vim /etc/mail.rc)，并添加如下内容
**这里以163邮箱为例**：
```
set from=xxx@163.com #发信人邮箱
set smtp=smtp.163.com # 163 smtp
set smtp-auth-user=xxx@163.com #接收人邮箱
set smtp-auth-password=ABCDEFG #授权码(授权码不等于邮箱密码)
set smtp-auth=login #认证方式
```

## 三、启动sendmail
```
systemctl start sendmail

```

## 四、通过mail发送邮件
```
echo '邮件内容' | mail -s '邮件标题' 收件人邮箱
或
mail -s '邮件标题' 收件人邮箱 < 邮件内容.txt
示例：
echo 'hello world' | mail -s 'hello world' xxxxxxxxxxx@163.com

```

## 五、邮箱发送原理图
理解原理，能更好的看清事物本质。
![](https://img-blog.csdnimg.cn/20181209152449164.png)
**简要概括:**
```
MUA：Mail User Agent，邮件用户代理，用来编写，收发邮件
MTA：Mail Transfer Agent，邮件传输代理，将邮件传输到正确目的地
MDA：Mail Delivery Agent，邮件分发代理，将邮件分发到正确目的用户

```

本文主要参考资料如下:
[linux利用mail和sendmail发送邮件](https://blog.csdn.net/u012219371/article/details/84929028)