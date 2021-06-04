---
title: Linux之监控微服务shell脚本
date: 2020-10-25 20:47:33
tags: "Linux"
---


监控微服务shell脚本内容(包含邮件告警):
<!--more-->
```

### check port
check_port() {

        netstat -tlpn | grep "\b$1\b"
}

### check mkdir
check_mkdir(){

 if [ ! -d "/home/youcong/project/monitor/$1" ]; then
      mkdir /home/youcong/project/monitor/$1
 fi

}

### server check 

monitor_server_register(){

if check_port $1                                  #端口

then
        
        DATE_N=`date "+%Y-%m%d"`
        
        DATE_N_F=`date "+%Y-%m%d %H:%M:%S"`        

        echo "server $1 online date:${DATE_N}" >> /home/youcong/project/monitor/$1/server_"${DATE_N}."log

    	exit 1
else
        

        DATE_N=`date "+%Y-%m%d"`

        DATE_N_F=`date "+%Y-%m%d %H:%M:%S"`


        echo "server $1 offline date:${DATE_N_F}" >> /home/youcong/project/monitor/$1/server_${DATE_N}.log

        echo "服务 $1 宕机 宕机日期为:${DATE_N_F} 可进入/home/youcong/project/log查看宕机时间或进入/home/youcong/project/log查看错误详情 " |mail -s "邮件告警-服务为$1 的端口宕机了" test@163.com 
fi
}

#服务端口(定义一个端口数组遍历监控,可写多个，记得以空格进行分隔)
arrayIndex=(8080 8081)

for var in ${arrayIndex[@]}
do   
     echo $var
     
     #检查目录是否存在
     check_mkdir $var &

     #检测微服务状态
     monitor_server_register $var &
     
done


```