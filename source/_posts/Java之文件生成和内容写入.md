---
title: Java之文件生成和内容写入
date: 2021-07-15 21:31:06
tags: "Java"
---

很久以前曾写过这样一篇文章:
[Java之创建文件并写入数据](https://blog.csdn.net/zhouzhiwengang/article/details/89031914)

今天这篇文章算是进化版的，还是说Java之文件生成和内容写入，只不过用的API不一样。
<!--more-->

```
import java.io.FileWriter;

public class FileUtil {

    public static void generateHtml() {
        try {
            String fileName = "test.html";
            String path = "D://usr//template//";
            FileWriter fw = new FileWriter(path + fileName, false);
            // FileWriter 如果文件名 的文件不存在，先创建再读写;存在的话直接追加写,关键字true表示追加
            String originalLine = "<h1>Blog-专属</h1>";
            //写入内容
            fw.write(originalLine);
            // 关闭写文件
            fw.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        generateHtml();
    }

}

```