---
title: JavaWeb之Response文件下载(中文编码问题)
date: 2021-06-03 20:23:32
tags: "Java"
---
很久以前遇到过这样的问题，最近再次遇到，做个记录。
<!--more-->
**核心代码如下(这里采用Excel导出是EasyPoi):**
```
 @RequestMapping("/downloadPost")
    public void downloadPost(HttpServletResponse response) {
        try {
            Workbook workbook = ExcelExportUtil.exportExcel(new ExportParams("博客园文章数据", "博客园文章数据"),
                    PostExcelEntity.class, postService.selectBasePostDataList());
            // 指定下载的文件名--设置响应头
            response.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode("博客园文章数据.xls", "UTF-8"));
            response.setContentType("application/vnd.ms-excel;charset=UTF-8");
            response.setHeader("Pragma", "no-cache");
            response.setHeader("Cache-Control", "no-cache");
            response.setDateHeader("Expires", 0);
            // 写出数据输出流到页面
            OutputStream output = response.getOutputStream();
            BufferedOutputStream bufferedOutPut = new BufferedOutputStream(output);
            workbook.write(bufferedOutPut);
            bufferedOutPut.flush();
            bufferedOutPut.close();
            output.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

```