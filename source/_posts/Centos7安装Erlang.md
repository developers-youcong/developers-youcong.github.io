---
title: Centos7安装Erlang
date: 2022-03-22 20:44:05
tags: "Linux"
---

## 一、yum安装
<!--more-->

```
//安装下载工具
yum install -y wget

//安装依赖项
yum install -y epel-release

//添加存储库条目
wget https://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
rpm -Uvh erlang-solutions-1.0-1.noarch.rpm

//安装
yum install -y erlang

//验证安装是否成功
erl -version

```

## 二、rpm安装
```
//安装依赖项
yum install -y epel-release

//下载rpm包
wget https://packages.erlang-solutions.com/erlang/rpm/centos/7/x86_64/esl-erlang_22.1-1~centos~7_amd64.rpm

//安装
yum install esl-erlang_22.1-1~centos~7_amd64.rpm

//验证安装是否成功
erl -version

```