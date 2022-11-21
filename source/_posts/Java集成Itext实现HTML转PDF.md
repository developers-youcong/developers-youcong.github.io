---
title: Java集成Itext实现HTML转PDF
date: 2021-07-15 21:25:31
tags: "Java"
---

## 一、导入Maven依赖
<!--more-->

```
 <!--集成itext -->
        <dependency>
            <groupId>com.itextpdf</groupId>
            <artifactId>itextpdf</artifactId>
            <version>5.5.13</version>
        </dependency>
        <dependency>
            <groupId>com.itextpdf.tool</groupId>
            <artifactId>xmlworker</artifactId>
            <version>5.5.13</version>
        </dependency>
        <dependency>
            <groupId>com.itextpdf</groupId>
            <artifactId>itext-asian</artifactId>
            <version>5.2.0</version>
        </dependency>

```

## 二、核心类
```
public class HtmlTransPDF {

    /**
     * @throws
     * @Title: htmlTransPdf
     * @Description: html 转 pdf ,简单字符和数字
     * @param: @param inputStream
     * @param: @param outputStream
     * @return: void
     */
    public static void htmlTransPdf(InputStream inputStream, OutputStream outputStream) {
        try {
            Document document = new Document();
            // 为该Document创建一个Writer实例
            PdfWriter pdfwriter = PdfWriter.getInstance(document,
                    outputStream);
            pdfwriter.setViewerPreferences(PdfWriter.HideToolbar);
            // 打开当前的document
            document.open();
            XMLWorkerHelper.getInstance().parseXHtml(pdfwriter, document, inputStream);
            document.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * @throws
     * @Title: htmlTransPdfChinese
     * @Description: html 转 pdf, 简单中文
     * @param: @param pdfFile
     * @param: @param content
     * @return: void
     */
    public static void htmlTransPdfChinese(String pdfFile, String content) {
        try {
            Document document = new Document();
            // 为该Document创建一个Writer实例
            PdfWriter pdfwriter = PdfWriter.getInstance(document,
                    new FileOutputStream(pdfFile));
            pdfwriter.setViewerPreferences(PdfWriter.HideToolbar);
            // 打开当前的document
            document.open();
            XMLWorkerHelper.getInstance().parseXHtml(pdfwriter, document, new ByteArrayInputStream(content.getBytes("Utf-8")), Charset.forName("UTF-8"));
            document.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
	}

}

```

## 三、测试
```
public static void main(String[] args) {
        String htmlPath = "D:\\usr\\template\\test.html";
        String pdfPath = "D:\\usr\\template\\test.pdf";
        String content = "";
        File htmlFile = new File(htmlPath);
        File pdfFile = new File(pdfPath);
        if(htmlFile.exists()){
            if(!pdfFile.exists()){
                try {
                    pdfFile.createNewFile();
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
            // 开始读取html 文件内容
            BufferedReader br;
            try {
                br = new BufferedReader(new InputStreamReader(
                        new FileInputStream(htmlFile), "UTF-8"));
                String row = "";
                while ((row = br.readLine()) != null) {
                    // System.out.println(t);
                    content += row;
                }
                HtmlTransPDF.htmlTransPdfChinese(pdfPath, content);
            } catch (UnsupportedEncodingException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (FileNotFoundException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }
```

参考资料:
[Java Itext 实现HTML 转换PDF](https://blog.csdn.net/zhouzhiwengang/article/details/89031914)