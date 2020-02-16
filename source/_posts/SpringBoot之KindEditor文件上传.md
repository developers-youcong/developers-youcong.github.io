---
title: SpringBoot之KindEditor文件上传
date: 2019-09-07 19:42:38
tags: "SpringBoot"
---

后端核心代码如下:
<!--more-->
```
package com.blog.springboot.controller;

import java.io.BufferedOutputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.io.PrintWriter;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.HashMap;
import java.util.Hashtable;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.UUID;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.tomcat.util.http.fileupload.servlet.ServletFileUpload;
import org.springframework.stereotype.Controller;
import org.springframework.util.FileCopyUtils;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.multipart.MultipartFile;
import org.springframework.web.multipart.MultipartHttpServletRequest;

import com.alibaba.fastjson.JSONObject;

@Controller
public class KindController {

    @RequestMapping(value = "/Kindeditor/uploadFile", method = RequestMethod.POST)
    public void uploadFile(HttpServletRequest request, HttpServletResponse response) throws Exception {
        PrintWriter writer = response.getWriter();
        // 文件保存目录路径
        String savePath = request.getSession().getServletContext().getRealPath("/") + "upload/image"
                + File.separatorChar + File.separatorChar;

        String saveUrl = request.getContextPath() + File.separatorChar + "upload/image" + File.separatorChar
                + File.separatorChar;

        // 定义允许上传的文件扩展名
        HashMap<String, String> extMap = new HashMap<String, String>();
        extMap.put("image", "gif,jpg,jpeg,png,bmp");

        // 最大文件大小
        long maxSize = 1000000;
        response.setContentType("text/html; charset=UTF-8");

        if (!ServletFileUpload.isMultipartContent(request)) {
            writer.println(getError("请选择文件。"));
            return;
        }

        File uploadDir = new File(savePath);
        // 判断文件夹是否存在,如果不存在则创建文件夹
        if (!uploadDir.exists()) {
            uploadDir.mkdirs();
        }

        // 检查目录写权限
        if (!uploadDir.canWrite()) {
            writer.println(getError("上传目录没有写权限。"));
            return;
        }

        String dirName = request.getParameter("dir");
        if (dirName == null) {
            dirName = "image";
        }
        if (!extMap.containsKey(dirName)) {
            writer.println(getError("目录名不正确。"));
            return;
        }

        MultipartHttpServletRequest mRequest = (MultipartHttpServletRequest) request;
        Map<String, MultipartFile> fileMap = mRequest.getFileMap();
        String fileName = null;
        for (Iterator<Map.Entry<String, MultipartFile>> it = fileMap.entrySet().iterator(); it.hasNext();) {
            Map.Entry<String, MultipartFile> entry = it.next();
            MultipartFile mFile = entry.getValue();
            fileName = mFile.getOriginalFilename();
            // 检查文件大小
            if (mFile.getSize() > maxSize) {
                writer.println(getError("上传文件大小超过限制。"));
                return;
            }
            String fileExt = fileName.substring(fileName.lastIndexOf(".") + 1);
            if (!Arrays.<String>asList(extMap.get(dirName).split(",")).contains(fileExt)) {
                writer.println(getError("上传文件扩展名是不允许的扩展名。\n只允许" + extMap.get(dirName) + "格式。"));
                return;
            }
            UUID uuid = UUID.randomUUID();
            String path = savePath + uuid.toString() + "." + fileExt;
            saveUrl = saveUrl + uuid.toString() + "." + fileExt;

            BufferedOutputStream outputStream = new BufferedOutputStream(new FileOutputStream(path));
            FileCopyUtils.copy(mFile.getInputStream(), outputStream);

            JSONObject obj = new JSONObject();
            obj.put("error", 0);
            obj.put("url", saveUrl);
            writer.println(obj.toString());

        }
    }

    private String getError(String message) {
        JSONObject obj = new JSONObject();
        obj.put("error", 1);
        obj.put("message", message);
        return obj.toString();
    }

    @RequestMapping(value = "/Kindeditor/fileManager", method = RequestMethod.GET)
    public void fileManager(HttpServletRequest request, HttpServletResponse response) throws Exception {
        // 根目录路径，可以指定绝对路径，比如 /var/www/attached/
        String rootPath = request.getSession().getServletContext().getRealPath("/") + "upload/";
        // 根目录URL，可以指定绝对路径，比如 http://www.yoursite.com/attached/
        String rootUrl = request.getContextPath() + "/upload/";
        // 图片扩展名
        String[] fileTypes = new String[] { "gif", "jpg", "jpeg", "png", "bmp" };
        System.out.println(rootPath);
        String dirName = request.getParameter("dir");
        if (dirName != null) {
            if (!Arrays.<String>asList(new String[] { "image", "flash", "media", "file" }).contains(dirName)) {
                System.out.println("Invalid Directory name.");
                return;
            }
            rootPath += dirName + "/";
            rootUrl += dirName + "/";
            File saveDirFile = new File(rootPath);
            if (!saveDirFile.exists()) {
                saveDirFile.mkdirs();
            }
        }
        // 根据path参数，设置各路径和URL
        String path = request.getParameter("path") != null ? request.getParameter("path") : "";
        String currentPath = rootPath + path;
        String currentUrl = rootUrl + path;
        String currentDirPath = path;
        String moveupDirPath = "";
        if (!"".equals(path)) {
            String str = currentDirPath.substring(0, currentDirPath.length() - 1);
            moveupDirPath = str.lastIndexOf("/") >= 0 ? str.substring(0, str.lastIndexOf("/") + 1) : "";
        }

        // 排序形式，name or size or type
        String order = request.getParameter("order") != null ? request.getParameter("order").toLowerCase() : "name";

        // 不允许使用..移动到上一级目录
        if (path.indexOf("..") >= 0) {
            System.out.println("Access is not allowed.");
            return;
        }
        // 最后一个字符不是/
        if (!"".equals(path) && !path.endsWith("/")) {
            System.out.println("Parameter is not valid.");
            return;
        }
        // 目录不存在或不是目录
        File currentPathFile = new File(currentPath);
        if (!currentPathFile.isDirectory()) {
            System.out.println("Directory does not exist.");
            return;
        }

        // 遍历目录取的文件信息
        List<Hashtable> fileList = new ArrayList<Hashtable>();
        if (currentPathFile.listFiles() != null) {
            for (File file : currentPathFile.listFiles()) {
                Hashtable<String, Object> hash = new Hashtable<String, Object>();
                String fileName = file.getName();
                if (file.isDirectory()) {
                    hash.put("is_dir", true);
                    hash.put("has_file", (file.listFiles() != null));
                    hash.put("filesize", 0L);
                    hash.put("is_photo", false);
                    hash.put("filetype", "");
                } else if (file.isFile()) {
                    String fileExt = fileName.substring(fileName.lastIndexOf(".") + 1).toLowerCase();
                    hash.put("is_dir", false);
                    hash.put("has_file", false);
                    hash.put("filesize", file.length());
                    hash.put("is_photo", Arrays.<String>asList(fileTypes).contains(fileExt));
                    hash.put("filetype", fileExt);
                }
                hash.put("filename", fileName);
                hash.put("datetime", new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(file.lastModified()));
                fileList.add(hash);
            }
        }

        if ("size".equals(order)) {
            Collections.sort(fileList, new SizeComparator());
        } else if ("type".equals(order)) {
            Collections.sort(fileList, new TypeComparator());
        } else {
            Collections.sort(fileList, new NameComparator());
        }
        JSONObject result = new JSONObject();
        result.put("moveup_dir_path", moveupDirPath);
        result.put("current_dir_path", currentDirPath);
        result.put("current_url", currentUrl);
        result.put("total_count", fileList.size());
        result.put("file_list", fileList);
        response.setContentType("application/json; charset=UTF-8");
        System.out.println(result.toJSONString());
        PrintWriter writer = response.getWriter();
        writer.println(result);
    }

    public class NameComparator implements Comparator {
        public int compare(Object a, Object b) {
            Hashtable hashA = (Hashtable) a;
            Hashtable hashB = (Hashtable) b;
            if (((Boolean) hashA.get("is_dir")) && !((Boolean) hashB.get("is_dir"))) {
                return -1;
            } else if (!((Boolean) hashA.get("is_dir")) && ((Boolean) hashB.get("is_dir"))) {
                return 1;
            } else {
                return ((String) hashA.get("filename")).compareTo((String) hashB.get("filename"));
            }
        }
    }

    public class SizeComparator implements Comparator {
        public int compare(Object a, Object b) {
            Hashtable hashA = (Hashtable) a;
            Hashtable hashB = (Hashtable) b;
            if (((Boolean) hashA.get("is_dir")) && !((Boolean) hashB.get("is_dir"))) {
                return -1;
            } else if (!((Boolean) hashA.get("is_dir")) && ((Boolean) hashB.get("is_dir"))) {
                return 1;
            } else {
                if (((Long) hashA.get("filesize")) > ((Long) hashB.get("filesize"))) {
                    return 1;
                } else if (((Long) hashA.get("filesize")) < ((Long) hashB.get("filesize"))) {
                    return -1;
                } else {
                    return 0;
                }
            }
        }
    }

    public class TypeComparator implements Comparator {
        public int compare(Object a, Object b) {
            Hashtable hashA = (Hashtable) a;
            Hashtable hashB = (Hashtable) b;
            if (((Boolean) hashA.get("is_dir")) && !((Boolean) hashB.get("is_dir"))) {
                return -1;
            } else if (!((Boolean) hashA.get("is_dir")) && ((Boolean) hashB.get("is_dir"))) {
                return 1;
            } else {
                return ((String) hashA.get("filetype")).compareTo((String) hashB.get("filetype"));
            }
        }
    }

}

```


前端核心代码:
```
var editor;
        KindEditor
                .ready(function(K) {
                    editor = K
                            .create(
                                    'textarea[id="postContent"]',
                                    {
                                        filterMode : false,
                                        allowFileManager : true,
                                        cssPath : [ '../../blog-web/static/vendor/kindeditor/plugins/code/prettify.css' ],
                                        filePostName : "file",
                                        //这里就是指定文件上传的请求地址，上面也已经说了，upload_json.jsp就相当去一个servlet去进行保存文件，这个地方很重要
                                        uploadJson : Blog.url.api.KindEditorUpload,
                                        fileManagerJson :Blog.url.api.KindEditorFileManager,
                                        resizeType : 1,
                                        allowPreviewEmoticons : true,
                                        allowImageUpload : true,
                                        filterMode : false,

                                    });

                });

        $(function() {
            prettyPrint();
        });

```

本文参考资料如下:

https://blog.csdn.net/u012131769/article/details/47430873(我参考这各地址实践成功的，希望能够对大家有所启发)

https://blog.csdn.net/kaisens/article/details/78586597