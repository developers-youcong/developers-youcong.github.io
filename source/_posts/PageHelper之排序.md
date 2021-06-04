---
title: PageHelper之排序
date: 2021-02-27 21:27:21
tags: "Java"
---

PageHelper是MyBatis的分页插件。关于MyBatis的分页插件如何使用和注意事项，可以参考我的这篇博客:
<!--more-->
[MyBatis分页插件失效问题之解决](https://youcongtech.com/2021/01/22/MyBatis%E5%88%86%E9%A1%B5%E6%8F%92%E4%BB%B6%E5%A4%B1%E6%95%88%E9%97%AE%E9%A2%98%E4%B9%8B%E8%A7%A3%E5%86%B3/)


今天说到的是利用Pagehelper排序，非常简单。

核心代码如下:
```
PageHelper.startPage(reqDTO.getCurPage(), reqDTO.getPageSize(), columAutoOrder(reqDTO.getOrderColumn(), reqDTO.getSort()));

```

其中最关键的核心方法，columAutoOrder(param1,param2)内容如下:
```
public static String getOrderBy(String orderByColumn, String sort) {

        if ("0".equals(sort)) {
            sort = "desc";
        }

        if ("1".equals(sort)) {
            sort = "asc";
        }
        return orderByColumn + " " + sort;
    }

```

合在一起完整代码如下:
```
    PageHelper.startPage(reqDTO.getCurPage(), reqDTO.getPageSize(), PageUtil.getOrderBy(reqDTO.getOrderColumn(), reqDTO.getSort()));

        BasePageVo<T> pageInfo = new BasePageVo(userMapper.selectUserList(companyCode);

```


BasePageVo.java:
```
@Data
@NoArgsConstructor
public class BasePageVo<T> {

    private List<T> pageList;
    private int curPage;
    private int pageSize;
    private long total;
    private int pages;

    public BasePageVo(List<T> list) {

        if (list instanceof Page) {
            Page page = (Page) list;
            this.curPage = page.getPageNum();
            this.pageSize = page.getPageSize();
            this.pages = page.getPages();
            this.pageList = page;
            this.total = page.getTotal();
        } else if (list instanceof Collection) {

            this.curPage = 1;
            this.pageSize = 10;
            this.pages = 1;
            this.pageList = list;
            this.total = (long) list.size();
        }


    }
}

```