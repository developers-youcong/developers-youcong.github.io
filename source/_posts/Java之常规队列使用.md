---
title: Java之常规队列使用
date: 2022-06-25 19:29:59
tags: "Java"
---

Java之常规队列的使用核心代码：
<!--more-->

```
import java.util.Iterator;
import java.util.Queue;
import java.util.Random;
import java.util.concurrent.*;


public class QueueTest {

    /**
     * 基于优先级阻塞队列
     */
    public static void priorityBlockingQueue() {
        //队列里元素必须实现Comparable接口,用来决定优先级
        PriorityBlockingQueue<String> pbq = new PriorityBlockingQueue<String>();
        pbq.add("a");
        pbq.add("b");
        pbq.add("c");
        pbq.add("d");
        //获取的时候会根据优先级取元素,插入的时候不会排序,节省性能
        //System.out.println(pbq.take());//a,获取时会排序,按优先级获取
        System.out.println(pbq.toString());//如果前面没有取值,直接syso也不会排序
        Iterator<String> iterator = pbq.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

    }


    /**
     * 基于链表阻塞队列
     */
    public static void linkedBlockingQueue() {
        Queue<UserVo> queue = new LinkedBlockingQueue<>();
        UserVo vo1 = new UserVo();
        vo1.setLoginAccount("testA");
        vo1.setNickName("testA");
        UserVo vo2 = new UserVo();
        vo2.setLoginAccount("testB");
        vo2.setNickName("testB");
        queue.offer(vo1);
        queue.offer(vo2);
        for (UserVo vo : queue) {
            System.out.println("vo:" + vo);
        }
    }

    /**
     * 基于链接节点的无界线程安全的队列
     */
    public static void concurrentLinkedQueue() {
        Queue<UserVo> queue = new ConcurrentLinkedQueue<>();
        UserVo vo1 = new UserVo();
        vo1.setLoginAccount("testA");
        vo1.setNickName("testA");
        UserVo vo2 = new UserVo();
        vo2.setLoginAccount("testB");
        vo2.setNickName("testB");
        queue.offer(vo1);
        queue.offer(vo2);
        for (UserVo vo : queue) {
            System.out.println("vo:" + vo);
        }
    }

    /**
     * 基于数组阻塞队列
     */
    public static void arrayBlockingQueue() {
        ArrayBlockingQueue<UserVo> blockingQueue = new ArrayBlockingQueue<UserVo>(2);
        UserVo vo1 = new UserVo();
        vo1.setLoginAccount("testA");
        vo1.setNickName("testA");
        UserVo vo2 = new UserVo();
        vo2.setLoginAccount("testB");
        vo2.setNickName("testB");
        blockingQueue.add(vo1);
        blockingQueue.add(vo2);
        if (blockingQueue.size() > 0) {
            for (UserVo vo : blockingQueue) {
                System.out.println("vo:" + vo);
            }
        }
    }

    /**
     * 特殊的阻塞队列
     *
     * @throws Exception
     */
    public static void synchronousQueue() throws Exception {
        SynchronousQueue<Integer> queue = new SynchronousQueue<Integer>();

        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try {
                        TimeUnit.SECONDS.sleep(1);
                        queue.put(new Random().nextInt(66));
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        }).start();

        while (true) {
            TimeUnit.SECONDS.sleep(1);
            System.out.println(queue.take());
        }

    }

    public static void main(String[] args) throws Exception{
        QueueTest.synchronousQueue();
    }

}


```