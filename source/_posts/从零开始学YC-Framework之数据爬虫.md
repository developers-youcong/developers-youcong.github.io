---
title: 从零开始学YC-Framework之数据爬虫
date: 2022-06-19 22:23:04
tags: "YC-Framework"
---

为了模拟一些真实的应用场景，需要一些真实的数据，一方面通过免费的API获取，另外一方面通过爬虫获取。YC-Framework针对一些特殊场景，提供了一套开源的数据爬虫方案，这些开源数据爬虫方案，均站在前辈的肩上。这些开源数据爬虫方案主要包含原生JSOUP、WebMagic等。YC-Framework以爬取博客园、思否、CSDN等内容网站作为案例。
<!--more-->

## 一、使用爬虫需要引入哪一个依赖？
```
<dependency>
    <groupId>com.yc.framework</groupId>
    <artifactId>yc-common-crawler</artifactId>
</dependency>

```

而yc-common-crawler依赖实际上是：
```
<properties>
    <webmagic.version>0.7.4</webmagic.version>
</properties>
<dependencies>
    <!-- webmagic-->
    <dependency>
        <groupId>us.codecraft</groupId>
        <artifactId>webmagic-core</artifactId>
        <version>${webmagic.version}</version>
        <exclusions>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
            <exclusion>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>us.codecraft</groupId>
        <artifactId>webmagic-extension</artifactId>
        <version>${webmagic.version}</version>
    </dependency>
</dependencies>

```

## 二、如何爬取博客园、CSDN、思否的内容数据？


### 1.博客园数据抓取核心代码
```
@Component
public class CnBlogDataCrawler implements PageProcessor {
    public static CnBlogDataCrawler cnBlogDataCrawler;

    @PostConstruct
    public void init() {
        cnBlogDataCrawler = this;
    }

    private Site site = Site.me()
            .setDomain("https://www.cnblogs.com/")
            .setSleepTime(1000)
            .setUserAgent("Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36");


    @Override
    @Transactional
    public void process(Page page) {
        Selectable obj = page.getHtml().xpath("//div[@class='post']");
        Selectable title = obj.xpath("//h1[@class='postTitle']//a");
        Selectable content = obj.xpath("//div[@class='blogpost-body']");
        System.out.println("title:" + title.replace("<[^>]*>", ""));
        System.out.println("content:" + content);
    }


    @Override
    public Site getSite() {
        return site;
    }


    /**
     * 导入单篇博客园文章数据
     *
     * @param url
     */
    public static void importSinglePost(String url) {
        Spider.create(new CnBlogDataCrawler())
                .addUrl(url)
                .addPipeline(new ConsolePipeline())
                .thread(10)
                .run();
    }
//
//    public static void main(String[] args) {
//        CnBlogDataCrawler.importSinglePost("https://www.cnblogs.com/afei654138148/p/15348696.html");
//    }
}


```

### 2.CSDN数据抓取核心代码
```
@Component
public class CSDNDataCrawler implements PageProcessor {

    public static CSDNDataCrawler csdnDataCrawler;

    @PostConstruct
    public void init() {
        csdnDataCrawler = this;
    }

    private Site site = Site.me()
            .setDomain("https://blog.csdn.net/")
            .setSleepTime(1000)
            .setUserAgent("Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36");

    @Override
    @Transactional
    public void process(Page page) {
        Selectable obj = page.getHtml().xpath("//div[@id='mainBox']");
        Selectable title = obj.xpath("//h1[@class='title-article']");
        Selectable content = obj.xpath("//div[@id='content_views']");
        System.out.println("title:" + title.replace("<[^>]*>", ""));
        System.out.println("content:" + content);
    }


    @Override
    public Site getSite() {
        return site;
    }


    /**
     * 导入单篇CSDN文章数据
     *
     * @param url
     */
    public static void importSingleCSDNPost(String url) {
        Spider.create(new CSDNDataCrawler())
                .addUrl(url)
                .addPipeline(new ConsolePipeline())
                .thread(8)
                .run();

    }

//    public static void main(String[] args) {
//        importSingleCSDNPost("https://blog.csdn.net/weixin_44318830/article/details/116402369?utm_medium=distribute.pc_feed.none-task-blog-whitelist_blog-7.nonecase&depth_1-utm_source=distribute.pc_feed.none-task-blog-whitelist_blog-7.nonecase");
//    }
}


```

### 3.思否数据抓取核心代码
```
@Component
public class SegmentfaultDataCrawler implements PageProcessor {


    public static SegmentfaultDataCrawler segmentfaultDataCrawler;

    @PostConstruct
    public void init() {
        segmentfaultDataCrawler = this;

    }

    private Site site = Site.me()
            .setDomain("https://segmentfault.com/")
            .setSleepTime(1000)
            .setUserAgent("Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36");

    @Override
    @Transactional
    public void process(Page page) {
        Selectable obj = page.getHtml().xpath("//div[@class='article-content']");
        Selectable title = obj.xpath("//h1[@class='h2 mb-3']//a");
        Selectable content = obj.xpath("//article[@class='article fmt article-content ']");
        System.out.println("title:" + title.replace("<[^>]*>", ""));
        System.out.println("content:" + content);
    }


    @Override
    public Site getSite() {
        return site;
    }


    /**
     * 导入单篇思否文章数据
     *
     * @param url
     */
    public static void importSingleSegmentFaultPost(String url) {
        Spider.create(new SegmentfaultDataCrawler())
                .addUrl(url)
                .addPipeline(new ConsolePipeline())
                .run();
    }

}


```

要想体验，直接运行对应的main方法即可，就能看到对应的文章数据输出，只要输出，基本上就能放到自己的数据库中，用于数据分析。


## 三、是否所有涉及数据爬虫相关的代码均放在yc-crawler？
是的。为了保持一种规范，所有的数据爬虫代码均会放入yc-crawler中。**但有一点特别要注意：**
爬虫所获取的数据一定不能商用，以及爬取对应的网站，应当适度获取，不能较为频繁的采集，否则容易违法。

上述源代码均已开源，开源不易，如果对你有帮助，不妨给个star！！！

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework