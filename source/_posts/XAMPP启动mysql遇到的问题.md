---
title: XAMPP启动mysql遇到的问题
date: 2019-06-12 17:58:43
tags: "PHP"
---
错误信息如下:
Version: '10.1.33-MariaDB'  socket: ''  port: 3389  mariadb.org binary distribution
2019-06-12 18:00:56 4a04 InnoDB: Warning: Using innodb_additional_mem_pool_size is DEPRECATED. This option may be removed in future releases, together with the option innodb_use_sys_malloc and with the InnoDB's internal memory allocator.
2019-06-12 18:00:56 18948 [Note] InnoDB: innodb_empty_free_list_algorithm has been changed to legacy because of small buffer pool size. In order to use backoff, increase buffer pool at least up to 20MB.

2019-06-12 18:00:56 18948 [Note] InnoDB: Using mutexes to ref count buffer pool pages
2019-06-12 18:00:56 18948 [Note] InnoDB: The InnoDB memory heap is disabled
2019-06-12 18:00:56 18948 [Note] InnoDB: Mutexes and rw_locks use Windows interlocked functions
2019-06-12 18:00:56 18948 [Note] InnoDB: _mm_lfence() and _mm_sfence() are used for memory barrier
2019-06-12 18:00:56 18948 [Note] InnoDB: Compressed tables use zlib 1.2.3
2019-06-12 18:00:56 18948 [Note] InnoDB: Using generic crc32 instructions
2019-06-12 18:00:56 18948 [Note] InnoDB: Initializing buffer pool, size = 16.0M
2019-06-12 18:00:56 18948 [Note] InnoDB: Completed initialization of buffer pool
2019-06-12 18:00:56 18948 [ERROR] InnoDB: \Program Files (x86)\xampp\mysql\data\ibdata1 can't be opened in read-write mode
2019-06-12 18:00:56 18948 [ERROR] InnoDB: The system tablespace must be writable!
2019-06-12 18:00:56 18948 [ERROR] Plugin 'InnoDB' init function returned error.
2019-06-12 18:00:56 18948 [ERROR] Plugin 'InnoDB' registration as a STORAGE ENGINE failed.
2019-06-12 18:00:56 18948 [Note] Plugin 'FEEDBACK' is disabled.
2019-06-12 18:00:56 18948 [ERROR] Unknown/unsupported storage engine: InnoDB
2019-06-12 18:00:56 18948 [ERROR] Aborting

解决方案:
<!--more-->
(1)打开任务管理器终止mysqld进程；

(2)打开mysql安装目录的data文件夹，删除以下2个文件：
 ib_logfile0和ib_logfile1

(3)重新启动mysql

 
参考解决办法:
[XAMPP启动mysql遇到的问题](https://www.cnblogs.com/mogujiang/p/5680441.html)
 
