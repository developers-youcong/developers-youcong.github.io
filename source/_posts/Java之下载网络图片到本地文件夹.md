---
title: Java之下载网络图片到本地文件夹
date: 2020-11-21 14:17:33
tags: "Java"
---

核心代码(下载网络图片到本地文件夹):
<!--more-->
```
public class DownFileUtils {
    public static void downloadFile(String remoteFilePath, String localFilePath) {
        URL urlfile = null;
        HttpURLConnection httpUrl = null;
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;
        File f = new File(localFilePath);
        try {
            urlfile = new URL(remoteFilePath);
            httpUrl = (HttpURLConnection) urlfile.openConnection();
            httpUrl.connect();
            bis = new BufferedInputStream(httpUrl.getInputStream());
            bos = new BufferedOutputStream(new FileOutputStream(f));
            int len = 2048;
            byte[] b = new byte[len];
            while ((len = bis.read(b)) != -1) {
                bos.write(b, 0, len);
            }
            bos.flush();
            bis.close();
            httpUrl.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                bis.close();
                bos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    public static void main(String[] args) {
   
//注意这个本地文件夹(localFileMkdir)必须要指定文件的名称如test.jpg，否则会出现下载失败(控制台会报错，访问拒绝)。 

DownFileUtils.downloadFile(networkImgUrl,localFileMkdir);

//  例子  DownFileUtils.downloadFile("http://www.test.com/img/2020.jpg","D:\\test\\test.jpg");
    
    }
}


```