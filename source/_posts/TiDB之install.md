---
title: TiDB之install
date: 2022-07-21 17:15:46
tags: ["TiDB","中间件","分布式"]
---

TiDB官方文档：
https://docs.pingcap.com/tidb/stable

本文内容均参考官方文档，读者朋友如果想尝试TiDB，以TiDB对应版本的安装教程为准。
<!--more-->

## 一、下载
```
curl --proto '=https' --tlsv1.2 -sSf https://tiup-mirrors.pingcap.com/install.sh | sh

```

## 二、运行最新版本集群
```
cd /root/.tiup/bin

tiup playground

```

成功结果输出如下:
```
The component `playground` version  is not installed; downloading from repository.
download https://tiup-mirrors.pingcap.com/playground-v1.10.2-linux-amd64.tar.gz 7.31 MiB / 7.31 MiB 100.00% 1.45 MiB/s                                       
Starting component `playground`: /root/.tiup/components/playground/v1.10.2/tiup-playground
Using the version v6.1.0 for version constraint "".
                
If you'd like to use a TiDB version other than v6.1.0, cancel and retry with the following arguments:
        Specify version manually:   tiup playground <version>
        Specify version range:      tiup playground ^5
        The nightly version:        tiup playground nightly

Playground Bootstrapping...
Start pd instance:v6.1.0
The component `pd` version v6.1.0 is not installed; downloading from repository.
download https://tiup-mirrors.pingcap.com/pd-v6.1.0-linux-amd64.tar.gz 43.25 MiB / 43.25 MiB 100.00% 1.20 MiB/s                                              
Start tikv instance:v6.1.0
The component `tikv` version v6.1.0 is not installed; downloading from repository.
download https://tiup-mirrors.pingcap.com/tikv-v6.1.0-linux-amd64.tar.gz 199.70 MiB / 199.70 MiB 100.00% 1.17 MiB/s                                          
Start tidb instance:v6.1.0
The component `tidb` version v6.1.0 is not installed; downloading from repository.
download https://tiup-mirrors.pingcap.com/tidb-v6.1.0-linux-amd64.tar.gz 51.48 MiB / 51.48 MiB 100.00% 1.19 MiB/s                                            
Waiting for tidb instances ready
127.0.0.1:4000 ... Done
The component `prometheus` version v6.1.0 is not installed; downloading from repository.
download https://tiup-mirrors.pingcap.com/prometheus-v6.1.0-linux-amd64.tar.gz 87.63 MiB / 87.63 MiB 100.00% 1.18 MiB/s                                      
download https://tiup-mirrors.pingcap.com/grafana-v6.1.0-linux-amd64.tar.gz 50.08 MiB / 50.08 MiB 100.00% 1.19 MiB/s                                         
Start tiflash instance:v6.1.0
The component `tiflash` version v6.1.0 is not installed; downloading from repository.
download https://tiup-mirrors.pingcap.com/tiflash-v6.1.0-linux-amd64.tar.gz 184.46 MiB / 184.46 MiB 100.00% 1.17 MiB/s                                       
Waiting for tiflash instances ready
127.0.0.1:3930 ... Done
CLUSTER START SUCCESSFULLY, Enjoy it ^-^
To connect TiDB: mysql --comments --host 127.0.0.1 --port 4000 -u root -p (no password)
To view the dashboard: http://127.0.0.1:2379/dashboard
PD client endpoints: [127.0.0.1:2379]
To view the Prometheus: http://127.0.0.1:9090
To view the Grafana: http://127.0.0.1:3000

```

## 三、通过客户端访问TiDB

### 1.使用TiUP Client连接
```
tiup client

```

### 2.使用MySQL连接
```
mysql --host 127.0.0.1 --port 4000 -u root

```