---
title: Java之异步调用的三种方式
date: 2022-04-25 21:58:53
tags: "Java"
---
Java异步调用较通用的三种方式！！！
<!--more-->

## 一、基于CompletableFuture
```

ExecutorService executor = Executors.newFixedThreadPool(2);

CompletableFuture<String> future = CompletableFuture.supplyAsync(new Supplier<String>() {
    @Override
    public String get() {
        System.out.println("开始执行任务!");
        try {
            //模拟耗时操作
            Thread.sleep(20000);
            System.out.println("任务执行中");
        } catch (Exception e) {
            e.printStackTrace();
        }
        return "耗时任务结束完毕!";
    }
}, executor);


future.thenAccept(e -> System.out.println(e + "success"));

```

## 二、基于Future
```
ExecutorService executor = Executors.newFixedThreadPool(1);
Future<Integer> future = executor.submit(new Callable<Integer>() {
    @Override
    public Integer call() throws Exception {
        System.out.println("===task start===");
        Thread.sleep(6000);
        System.out.println("===task finish===");
        return 3;
    }
});
//这里需要返回值时会阻塞主线程，如果不需要返回值使用是OK的。倒也还能接收
Integer result=future.get();
System.out.println("result:"+result);
System.out.println("main函数执行结束");
System.in.read();

```

## 三、基于@Async

### 1.核心配置类
```
@EnableAsync
public class AsyncConfig {

    @Bean
    public TaskExecutor executor(){
        ThreadPoolTaskExecutor executor=new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10); //核心线程数
        executor.setMaxPoolSize(20);  //最大线程数
        executor.setQueueCapacity(1000); //队列大小
        executor.setKeepAliveSeconds(300); //线程最大空闲时间
        executor.setThreadNamePrefix("fsx-Executor-"); //指定用于新创建的线程名称的前缀。
        executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        return executor;
    }
}


```

### 2.测试类
```
public class TestSync {

@Async
public void test1() {
    System.out.println("执行任务中");
    try {
        Thread.sleep(10000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.println("完成");

}

@Async
public Future<Integer> test2() {
    System.out.println("执行任务中");

    try {
        Thread.sleep(10000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    System.out.println("完成");
    return new AsyncResult<>(3);
}

public static void main(String[] args) {
    TestSync sync= new TestSync();
    sync.test2();
}


}

```