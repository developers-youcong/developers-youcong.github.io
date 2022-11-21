---
title: GmSSL Project分享
date: 2022-09-03 21:12:33
tags: ["开源项目","密码工具箱"]
---

## 一、GmSSL是什么？
<!--more-->
GmSSL是一个开源的密码工具箱，支持SM2/SM3/SM4/SM9/ZUC等国密(国家商用密码)算法、SM2国密数字证书及基于SM2证书的SSL/TLS安全通信协议，支持国密硬件密码设备，提供符合国密规范的编程接口与命令行工具，可以用于构建PKI/CA、安全通信、数据加密等符合国密标准的安全应用。GmSSL项目是OpenSSL项目的分支，并与OpenSSL保持接口兼容。因此GmSSL可以替代应用中的OpenSSL组件，并使应用自动具备基于国密的安全能力。GmSSL项目采用对商业应用友好的类BSD开源许可证，开源且可以用于闭源的商业应用。

GmSSL项目由北京大学关志副研究员的密码学研究组开发维护，项目源码托管于GitHub。自2014年发布以来，GmSSL已经在多个项目和产品中获得部署与应用，并获得2015年度“一铭杯”中国Linux软件大赛二等奖(年度最高奖项)与开源中国密码类推荐项目。GmSSL项目的核心目标是通过开源的密码技术推动国内网络空间安全建设。


## 二、GmSSL有哪些关键特性？
- 1.支持SM2/SM3/SM4/SM9/ZUC等全部已公开国密算法。
- 2.支持国密SM2双证书SSL套件和国密SM9标识密码套件。
- 3.高效实现在主流处理器上可完成4.5万次SM2签名。
- 4.支持动态接入具备SKF/SDF接口的硬件密码模块。
- 5.支持门限签名、秘密共享和白盒密码等高级安全特性。
- 6.支持Java、Go、PHP等多语言接口绑定和REST服务接口。

## 三、GmSSL包含哪些相对独立的子项目？
- 1.国密Nginx：提供支持国密SSL和证书的Nginx服务器。
- 2.国密浏览器：基于Chromium的支持国密SSL和证书的通用浏览器。
- 3.SDF ENGINE：支持提供SDF接口的PCI-E密码卡和服务器密码机。
- 4.SKF ENGINE：支持提供SKF接口的USB-KEY、TF卡等智能密码钥匙。
- 5.SM9密钥服务：为个人用户提供基于电子邮件地址的SM9标识密钥服务。
- 6.自助CA服务：提供自助式的网站DV证书服务和S/MIME邮件证书服务。
- 7.国密标准文本：提供全面的国密GM/T系列标准文本。

## 四、GmSSL3.0相对2.0从哪些方向进行了改进？
- 1.采用CMake替代目前基于Perl的构建系统。
- 2.支持Linux/Windows/macOS/Android/iOS等主流操作系统，移除对嵌入式OS等其他系统的支持。
- 3.支持X86/ARM/RISC-V，针对上述平台64位指令集做汇编层面的优化。
- 4.将C语言标准由目前的C89更新为最新的C99或C11，及部分GCC特性，移除对Perl的依赖。
- 5.移除不安全的算法和协议，仅支持国密算法和主流国际算法，提升对AEAD、TLS 1.3等新标准的默认支持力度。
- 6.提升密码算法抗木马、抗侧信道攻击的安全性。
- 7.降低运行时堆内存的使用量，降低总体二进制代码体积。
- 8.提供特定于国密算法和协议的统一的多语言（支持Rust/Java/Go/PHP）封装。
- 9.保持和OpenSSL最新版本的兼容性，实现GmSSL和OpenSSL在同一个软件中的共存。


## 五、关于GmSSL的相关资料有哪些？
GmSSL官方网站:
http://gmssl.org/

快速上手:
http://gmssl.org/docs/quickstart.html

项目文档:
http://gmssl.org/docs/docindex.html

最新动态:
http://gmssl.org/news/newsindex.html

下载:
https://github.com/guanzhi/GmSSL/releases

关于开发者相关信息:
http://gmssl.org/members.html


北京大学信息安全实验室:
http://infosec.pku.edu.cn/

开源中国关于OpenSSL相关介绍:
https://www.oschina.net/p/GmSSL

Github源代码(国产项目开源不易，给个star鼓励一下):
https://github.com/guanzhi/GmSSL/