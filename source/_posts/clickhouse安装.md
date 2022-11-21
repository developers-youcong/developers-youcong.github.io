---
title: clickhouse安装
date: 2022-08-18 15:24:53
tags: ["YC-Framework","中间件","分布式"]
---

## 一、下载与安装
<!--more-->

```
curl https://clickhouse.com/ | sh

sudo ./clickhouse install

```


控制台输出(中间会弹出一个让你输密码的提示):
```
Copying ClickHouse binary to /usr/bin/clickhouse.new
Renaming /usr/bin/clickhouse.new to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-server to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-client to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-local to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-benchmark to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-copier to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-obfuscator to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-git-import to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-compressor to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-format to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-extract-from-config to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-keeper to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-keeper-converter to /usr/bin/clickhouse.
Creating symlink /usr/bin/clickhouse-disks to /usr/bin/clickhouse.
Creating clickhouse group if it does not exist.
 groupadd -r clickhouse
Creating clickhouse user if it does not exist.
 useradd -r --shell /bin/false --home-dir /nonexistent -g clickhouse clickhouse
Will set ulimits for clickhouse user in /etc/security/limits.d/clickhouse.conf.
Creating config directory /etc/clickhouse-server.
Creating config directory /etc/clickhouse-server/config.d that is used for tweaks of main server configuration.
Creating config directory /etc/clickhouse-server/users.d that is used for tweaks of users configuration.
Data path configuration override is saved to file /etc/clickhouse-server/config.d/data-paths.xml.
Log path configuration override is saved to file /etc/clickhouse-server/config.d/logger.xml.
User directory path configuration override is saved to file /etc/clickhouse-server/config.d/user-directories.xml.
OpenSSL path configuration override is saved to file /etc/clickhouse-server/config.d/openssl.xml.
Creating log directory /var/log/clickhouse-server.
Creating data directory /var/lib/clickhouse.
Creating pid directory /var/run/clickhouse-server.
 chown -R clickhouse:clickhouse '/var/log/clickhouse-server'
 chown -R clickhouse:clickhouse '/var/run/clickhouse-server'
 chown  clickhouse:clickhouse '/var/lib/clickhouse'
Enter password for default user: 
Password for default user is saved in file /etc/clickhouse-server/users.d/default-password.xml.
Setting capabilities for clickhouse binary. This is optional.
Cannot set 'net_admin' or 'ipc_lock' or 'sys_nice' or 'net_bind_service' capability for clickhouse binary. This is optional. Taskstats accounting will be disabled. To enable taskstats accounting you may add the required capability later manually.
Allow server to accept connections from the network (default is localhost only), [y/N]: y
The choice is saved in file /etc/clickhouse-server/config.d/listen.xml.
 chown -R clickhouse:clickhouse '/etc/clickhouse-server'

ClickHouse has been successfully installed.

```

## 二、启动clickhouse服务
```
 sudo clickhouse start


```
控制台输出:
```
chown -R clickhouse: '/var/run/clickhouse-server/'
Will run clickhouse su 'clickhouse' /usr/bin/clickhouse-server --config-file /etc/clickhouse-server/config.xml --pid-file /var/run/clickhouse-server/clickhouse-server.pid --daemon
Waiting for server to start
Waiting for server to start
Server started

```


## 三、访问clickhouse服务
```
clickhouse-client --password

```

参考资料如下:
https://clickhouse.com/docs/en/quick-start