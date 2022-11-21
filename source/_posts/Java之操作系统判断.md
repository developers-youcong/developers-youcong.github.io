---
title: Java之操作系统判断
date: 2022-10-23 10:41:08
tags: "Java"
---



基于Java原生API编写而成的操作系统判断工具类:

<!--more-->
```
public class JudgeSysUtil {

    private static final String OS_NAME = "os.name";

    private static final String LINUX_SYS = "linux";

    private static final String WINDOWS_SYS = "windows";

    private static final String OTHER_SYS = "other system";

    /**
     * Linux
     *
     * @return
     */
    public static boolean isLinux() {
        return System.getProperty(OS_NAME).toLowerCase().contains("linux");
    }

    /**
     * Windows
     *
     * @return
     */
    public static boolean isWindows() {
        return System.getProperty(OS_NAME).toLowerCase().contains("windows");
    }

    /**
     * 判断当前操作系统
     *
     * @return
     */
    public static String JudgeSystem() {
        if (isLinux()) {
            return LINUX_SYS;
        } else if (isWindows()) {
            return WINDOWS_SYS;
        } else {
            return OTHER_SYS;
        }
    }

    public static void main(String[] args) {
        System.out.println("current system is:" + JudgeSysUtil.JudgeSystem());
    }
}


```



**该工具类主要解决的问题如下:**

- 1.开发环境与部署环境的自动化切换。
- 2.开发环境与部署环境读取不同的配置文件。
- 3.针对不同操作系统下的服务进行定制化以及可能兼容。

