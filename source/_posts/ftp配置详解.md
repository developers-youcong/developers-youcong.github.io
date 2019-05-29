---
title: ftp配置详解
date: 2019-04-20 21:21:57
tags: "Linux"
---
## FTP配置详解
FTP配置文件位置/etc/vsftpd.conf
listen=NO
设置为YES时vsftpd以独立运行方式启动，设置为NO时以xinetd方式启动（xinetd是管理守护进程的，将服务集中管理，可以减少大量服务的资源消耗）
listen_ipv6=YES
以上两个只能一个YES一个NO否则会出错
listen_port=port
设置控制连接的监听端口号，默认为21
listen_address=ip_address
将在绑定到指定IP地址运行，适合多网卡
connect_from_port_20=YES/NO
若为YES，则强迫FTP－DATA的数据传送使用port 20，默认YES
anonymous_enable=YES
<!--more-->
允许匿名登陆
anon_root=/home/ftp
匿名登陆进去后的默认目录，这个自己设置
no_anon_password=YES
匿名登陆不需要密码
anon_upload_enable=YES
匿名用户是否能够上传文件，这个YES表示允许，并且它的父目录要有可写权限
anon_mkdir_write_enable=YES
允许匿名用户创建目录
anon_other_write_enable=NO
不允许匿名用户具有建立目录，上传之外的权限
anon_max_rate=n
设置匿名用户的最大传输速率，单位为B/s，值为0表示不限制
write_enable=YES
登陆用户是否有写权限，全局设置
local_enable=YES
是否允许本地用户登陆
local_root=/../..
本地登陆后的默认目录
控制用户访问文件vsftpd.user_list(文件中一行一个用户名)
在/etc/下面，没有就自己新建一个
userlist_file=/../..
上面那个文件的路径
userlist_enable=YES
是否启动vsftpd.user_list这个文件
userlist_deny=YES/NO
当为YES的时候，在vsftpd.user_list中的用户名不能登陆FTP
当为NO的时候，只有vsftpd.user_list中的用户名能登陆FTP
idle_session_timeout=300
设置多长时间不对FTP服务器进行任何操作，则断开该FTP连接，单位为秒
accept_timeout=60
建立FTP连接超时时间，单位秒
connection_timeout=60
PORT方式下建立FTP数据连接超时时间，单位秒
data_connection_timeout=60
设置空闲的用户会话在N秒后中断，单位秒
xferlog_enable=YES
开启日志记录
xferlog_file=/var/log/vsftpd.log
设置日志文件路径
pasv_enable=YES/NO
是否开启被动模式进行数据传输，有的客户机在防火墙后面，所以建议开启
pasv_min_port=n
pasv_max_port=m
设置被动模式后的数据连接端口范围在n和m之间
max_clients=n
在独立启动时限制服务器的连接数，0表示无限制
FTP添加用户
useradd函数，用于添加ftp用户
参数：
-d 指定用户根目录
-s 指定shell脚本为/bin/bash
-g 创建分组ftp分组
-G 指定root分组
例如:
useradd -d /home/linux/myftp -s /bin/bash ftpuser
详细信息输入useradd -h查看
删除ftp用户和主目录 userdel -r youruser
详细信息输入userdel -h查看
设置ftp用户密码
passwd ftpuser
输入命令后会让你输密码的
如果ftpuser是已存在的用户，则为修改旧密码
修改完配置文件后一定要重启服务
sudo /etc/init.d/vsftpd restart

参考链接：https://www.jianshu.com/p/a299650780e0

