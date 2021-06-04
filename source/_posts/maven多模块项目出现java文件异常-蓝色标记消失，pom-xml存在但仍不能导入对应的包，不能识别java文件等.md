---
title: maven多模块项目出现java文件异常(蓝色标记消失，pom.xml存在但仍不能导入对应的包，不能识别java文件等)
date: 2020-09-05 15:58:30
tags: "idea"
---
解决方案:
A->删除.idea文件，重新导入Idea
B->删除整个项目，重新下载导入Idea
C->Project Structure Module(对应的module)重新设置source root
D->检查pom.xml是否出错，如主pom.xml中的<modules>标签是否包含对应的子模块等
