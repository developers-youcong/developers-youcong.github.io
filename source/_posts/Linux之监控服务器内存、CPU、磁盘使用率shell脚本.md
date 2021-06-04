---
title: Linux之监控服务器内存、CPU、磁盘使用率shell脚本
date: 2020-10-25 20:53:41
tags: "Linux"
---

监控服务器内存、CPU、磁盘使用率脚本内容(包含邮件告警):
<!--more-->
```
#Memory
totalMemory=$(free -m | awk -F '[ :]+' 'NR==2{print $2}')
usedMemory=$(free -m | awk -F '[ :]+' 'NR==2{print $3}')
freeMemory=$(free -m|awk  '{print $4}'|sed -n '3p')
usedPerMemory=$(awk 'BEGIN{printf "%.0f",('$usedMemory'/'$totalMemory')*100}')
freePerMemory=$(awk 'BEGIN{printf "%.0f",('$freeMemory'/'$totalMemory')*100}')
logFile=/tmp/monitor.log
#alerm date
now_time=`date '+%Y-%m%d %H:%M:%S'`
#Disk
diskusAge=$(df -h | grep /dev/mapper/centos-root | awk -F '[ %]+' '{print $5}')

#CPU
cpuusAge=$(top -b -n 1 |grep Cpu|awk '{print $8}'|cut -f 1 -d'.')

echo $cpuusAge


# send mail
function send_mail(){
        mail -s "test alerm" test@163.com < /tmp/monitor.log
}


if [[ "$usedPerMemory" > 90 ]] || [[ "$diskusAge" > 90 ]] || [[ "$cpuusAge" > 95 ]];then

                echo "报警时间：${now_time}" > $logFile
                echo "CPU usage:${cpuusAge}%" >> $logFile
                echo "Memory usage:${usedPerMemory}%" >> $logFile
                echo "Disk usage:${diskusAge}%" >> $logFile
                send_mail
fi


```