---
title: Java之操作SCP
date: 2022-03-16 18:59:26
tags: "Java"
---

Java操作SCP的场景很多，如下:

- 1.从B服务器拉取数据文件，读取并存储到数据库或其它存储介质；
- 2.特定时间将数据文件从A服务器上传到C服务器，便于C服务器特定运算数据文件并存储到数据库或其它存储介质。

<!--more-->


## 一、导入Maven依赖
```
<dependency>
    <groupId>org.apache.sshd</groupId>
    <artifactId>sshd-scp</artifactId>
    <version>2.1.0</version>
</dependency>
<dependency>
    <groupId>ch.ethz.ganymed</groupId>
    <artifactId>ganymed-ssh2</artifactId>
    <version>build210</version>
</dependency>

```

## 二、编写核心方法
```
 /**
     * 从本地上传至服务器
     *
     * @param userName
     * @param password
     * @param ipAddr
     * @param loadFilePath
     * @param serverPath
     * @return
     */
    public static boolean localToServer(String userName, String password, String ipAddr, String loadFilePath, String serverPath) {
        boolean isAuthed = false;
        try {
            if (InetAddress.getByName(ipAddr).isReachable(1500)) {
                Connection conn = new Connection(ipAddr);
                conn.connect();
                isAuthed = conn.authenticateWithPassword(userName, password);
                if (isAuthed) {
                    SCPClient scpClient = conn.createSCPClient();
                    scpClient.put(loadFilePath, serverPath);
                    conn.close();
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        return isAuthed;
    }

    /**
     * 从服务器拉取文件到本地
     *
     * @param userName
     * @param password
     * @param ipAddr
     * @param serverPath
     * @param localPath
     * @return
     */
    public static boolean serverToLocal(String userName, String password, String ipAddr, String serverPath, String localPath) {
        boolean isAuthed = false;
        boolean status = false;
        try {
            status = InetAddress.getByName(ipAddr).isReachable(1500);
            System.out.println(status);
            if (status) {
                Connection conn = new Connection(ipAddr);
                conn.connect();
                isAuthed = conn.authenticateWithPassword(userName, password);
                System.out.println(isAuthed);
                if (isAuthed) {
                    Session session = conn.openSession();
                    SCPClient scpClient = conn.createSCPClient();
                    scpClient.get(serverPath, localPath);
                    session.close();
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        return isAuthed;
    }


```

## 三、简单测试
```
public static void main(String[] args) {
        // localToServer("tech","zxcvb","192.168.10.2","D://Tools//test.txt","/home/tech/test/");
        serverToLocal("tech", "zxcvb", "192.168.10.2", "/home/tech/test/***", "D://Tools//test//");
    }

```