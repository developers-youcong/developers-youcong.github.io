---
title: 巧用Arthas排查JVM
date: 2022-03-22 19:12:05
tags: "Java"
---

## 一、下载Arthas
```
curl -O https://arthas.aliyun.com/arthas-boot.jar

```
<!--more-->

## 二、运行Arthas
```
java -jar arthas-boot.jar

```

输出结果如下:
```
[INFO] arthas-boot version: 3.5.5
[INFO] Found existing java process, please choose one and input the serial number of the process, eg : 1. Then hit ENTER.
* [1]: 771303 kafka.Kafka
  [2]: 770583 org.apache.zookeeper.server.quorum.QuorumPeerMain
  

```

## 三、输入1
显示如下输出:
```
[INFO] Start download arthas from remote server: https://arthas.aliyun.com/download/3.5.6?mirror=aliyun
[INFO] File size: 12.94 MB, downloaded size: 1.85 MB, downloading ...
[INFO] File size: 12.94 MB, downloaded size: 3.47 MB, downloading ...
[INFO] File size: 12.94 MB, downloaded size: 4.69 MB, downloading ...
[INFO] File size: 12.94 MB, downloaded size: 5.86 MB, downloading ...
[INFO] File size: 12.94 MB, downloaded size: 7.04 MB, downloading ...
[INFO] File size: 12.94 MB, downloaded size: 8.23 MB, downloading ...
[INFO] File size: 12.94 MB, downloaded size: 9.41 MB, downloading ...
[INFO] File size: 12.94 MB, downloaded size: 10.58 MB, downloading ...
[INFO] File size: 12.94 MB, downloaded size: 11.74 MB, downloading ...
[INFO] File size: 12.94 MB, downloaded size: 12.89 MB, downloading ...
[INFO] Download arthas success.
[INFO] arthas home: /root/.arthas/lib/3.5.6/arthas
[INFO] Try to attach process 771303
[INFO] Found java home from System Env JAVA_HOME: /home/tech/jdk1.8.0_251
[INFO] Attach process 771303 success.
[INFO] arthas-client connect 127.0.0.1 3658
  ,---.  ,------. ,--------.,--.  ,--.  ,---.   ,---.                           
 /  O  \ |  .--. ''--.  .--'|  '--'  | /  O  \ '   .-'                          
|  .-.  ||  '--'.'   |  |   |  .--.  ||  .-.  |`.  `-.                          
|  | |  ||  |\  \    |  |   |  |  |  ||  | |  |.-'    |                         
`--' `--'`--' '--'   `--'   `--'  `--'`--' `--'`-----'                          

wiki       https://arthas.aliyun.com/doc                                        
tutorials  https://arthas.aliyun.com/doc/arthas-tutorials.html                  
version    3.5.6                                                                
main_class                                                                      
pid        771303                                                               
time       2022-03-22 19:10:31   

```

## 四、输入dashboard
```
ID     NAME                                   GROUP              PRIORITY      STATE        %CPU         DELTA_TIME   TIME         INTERRUPTED  DAEMON       
-1     C2 CompilerThread0                     -                  -1            -            1.21         0.060        0:18.742     false        true         
-1     C1 CompilerThread1                     -                  -1            -            0.23         0.011        0:14.984     false        true         
78     Timer-for-arthas-dashboard-ef594071-fe system             5             RUNNABLE     0.13         0.006        0:0.077      false        true         
76     arthas-NettyHttpTelnetBootstrap-3-2    system             5             RUNNABLE     0.07         0.003        0:0.101      false        true         
14     metrics-meter-tick-thread-2            main               5             WAITING      0.02         0.001        28:52.213    false        true         
-1     VM Periodic Task Thread                -                  -1            -            0.02         0.001        21:16.386    false        true         
13     metrics-meter-tick-thread-1            main               5             TIMED_WAITIN 0.02         0.001        28:54.511    false        true         
-1     Unknown Thread                         -                  -1            -            0.02         0.000        45:9.534     false        true         
41     ExpirationReaper-0-topic               main               5             TIMED_WAITIN 0.01         0.000        5:14.090     false        false        
60     data-plane-kafka-network-thread-0-List main               5             RUNNABLE     0.01         0.000        4:49.004     false        false        
58     data-plane-kafka-network-thread-0-List main               5             RUNNABLE     0.01         0.000        4:53.110     false        false        
59     data-plane-kafka-network-thread-0-List main               5             RUNNABLE     0.01         0.000        4:51.080     false        false        
34     ExpirationReaper-0-ElectLeader         main               5             TIMED_WAITIN 0.01         0.000        5:19.406     false        false        
61     data-plane-kafka-socket-acceptor-Liste main               5             RUNNABLE     0.01         0.000        2:17.763     false        false        
47     ExpirationReaper-0-AlterAcls           main               5             TIMED_WAITIN 0.01         0.000        5:27.389     false        false        
42     ExpirationReaper-0-Heartbeat           main               5             TIMED_WAITIN 0.01         0.000        5:47.303     false        false        
48     data-plane-kafka-request-handler-0     main               5             TIMED_WAITIN 0.01         0.000        3:57.953     false        true         
31     ExpirationReaper-0-Produce             main               5             TIMED_WAITIN 0.01         0.000        5:26.532     false        false        
56     data-plane-kafka-request-handler-7     main               5             TIMED_WAITIN 0.0          0.000        3:59.823     false        true         
Memory                            used       total      max         usage      GC                                                                            
heap                              193M       1024M      1024M       18.87%     gc.g1_young_generation.count           65                                     
g1_eden_space                     42M        637M       -1          6.59%      gc.g1_young_generation.time(ms)        431                                    
g1_survivor_space                 8M         8M         -1          100.00%    gc.g1_old_generation.count             0                                      
g1_old_gen                        143M       379M       1024M       13.99%     gc.g1_old_generation.time(ms)          0                                      
nonheap                           68M        71M        -1          95.67%                                                                                   
code_cache                        12M        12M        240M        5.16%                                                                                    
metaspace                         49M        52M        -1          94.99%                                                                                   
compressed_class_space            6M         6M         1024M       0.61%                                                                                    
direct                            16K        16K        -           100.01%                                                                                  
mapped                            0K         0K         -           0.00%                                                                                    
Runtime                                                                                                                                                      
os.name                                                                        Linux                                                                         
os.version                                                                     4.18.0-240.el8.x86_64                                                         
java.version                                                                   1.8.0_251                                                                     
java.home                                                                      /home/tech/jdk1.8.0_251/jre                                                   
systemload.average                                                             0.09                                                                          
processors                                                                     2                                                                             
timestamp/uptime                                                               Mon Mar 22 19:15:40 CST 2022/5365446s                                         
ID     NAME                                   GROUP              PRIORITY      STATE        %CPU         DELTA_TIME   TIME         INTERRUPTED  DAEMON       
-1     C2 CompilerThread0                     -                  -1            -            0.49         0.024        0:18.767     false        true         
-1     C1 CompilerThread1                     -                  -1            -            0.13         0.006        0:14.991     false        true         
78     Timer-for-arthas-dashboard-ef594071-fe system             5             RUNNABLE     0.12         0.005        0:0.083      false        true         
76     arthas-NettyHttpTelnetBootstrap-3-2    system             5             RUNNABLE     0.06         0.003        0:0.104      false        true         
-1     VM Periodic Task Thread                -                  -1            -            0.03         0.001        21:16.387    false        true         
14     metrics-meter-tick-thread-2            main               5             WAITING      0.02         0.001        28:52.214    false        true         
13     metrics-meter-tick-thread-1            main               5             TIMED_WAITIN 0.02         0.000        28:54.511    false        true         
-1     Unknown Thread                         -                  -1            -            0.01         0.000        45:9.535     false        true         
47     ExpirationReaper-0-AlterAcls           main               5             TIMED_WAITIN 0.01         0.000        5:27.390     false        false        
41     ExpirationReaper-0-topic               main               5             TIMED_WAITIN 0.01         0.000        5:14.090     false        false        
42     ExpirationReaper-0-Heartbeat           main               5             TIMED_WAITIN 0.01         0.000        5:47.303     false        false        
32     ExpirationReaper-0-Fetch               main               5             TIMED_WAITIN 0.01         0.000        5:13.788     false        false        
33     ExpirationReaper-0-DeleteRecords       main               5             TIMED_WAITIN 0.01         0.000        5:14.990     false        false        
34     ExpirationReaper-0-ElectLeader         main               5             TIMED_WAITIN 0.01         0.000        5:19.406     false        false        
31     ExpirationReaper-0-Produce             main               5             TIMED_WAITIN 0.01         0.000        5:26.532     false        false        
58     data-plane-kafka-network-thread-0-List main               5             RUNNABLE     0.01         0.000        4:53.111     false        false        
43     ExpirationReaper-0-Rebalance           main               5             TIMED_WAITIN 0.01         0.000        5:19.459     false        false        
60     data-plane-kafka-network-thread-0-List main               5             RUNNABLE     0.01         0.000        4:49.004     false        false        
59     data-plane-kafka-network-thread-0-List main               5             RUNNABLE     0.01         0.000        4:51.080     false        false        
Memory                            used       total      max         usage      GC                                                                            
heap                              193M       1024M      1024M       18.87%     gc.g1_young_generation.count           65                                     
g1_eden_space                     42M        637M       -1          6.59%      gc.g1_young_generation.time(ms)        431                                    
g1_survivor_space                 8M         8M         -1          100.00%    gc.g1_old_generation.count             0                                      
g1_old_gen                        143M       379M       1024M       13.99%     gc.g1_old_generation.time(ms)          0                                      
nonheap                           68M        71M        -1          95.68%                                                                                   
code_cache                        12M        12M        240M        5.18%                                                                                    
metaspace                         49M        52M        -1          95.00%                                                                                   
compressed_class_space            6M         6M         1024M       0.61%                                                                                    
direct                            16K        16K        -           100.01%                                                                                  
mapped                            0K         0K         -           0.00%                                                                                    
Runtime                                                                                                                                                      
os.name                                                                        Linux                                                                         
os.version                                                                     4.18.0-240.el8.x86_64                                                         
java.version                                                                   1.8.0_251                                                                     
java.home                                                                      /home/tech/jdk1.8.0_251/jre                                                   
systemload.average                                                             0.16                                                                          
processors                                                                     2                                                                             
timestamp/uptime                                                               Mon Mar 22 19:15:45 CST 2022/5365451s                   

```

## 五、Ctrl+Z终止dashboard

## 六、exit退出


参考资料:
https://arthas.aliyun.com/doc/