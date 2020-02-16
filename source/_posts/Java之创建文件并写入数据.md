---
title: Java之创建文件并写入数据
date: 2019-09-23 16:18:00
tags: "Java"
---


应用场景:
以OJ项目为例，创建对应的.in或.out文件，并将相关的数据写入。
<!--more-->
核心代码如下:
```
	
	/**
	 * 创建文件
	 * @param data
	 * @param basedir
	 * @param name
	 */
	public void createFile(String data, String basedir, String name) {
		try {
				File file = new File(basedir + "/" + name);
		        boolean b = file.createNewFile();
		        if(b) {
		        	Writer out = new FileWriter(file);
					out.write(data);
					out.close();
		        }
		        
		}catch (Exception e) {
			e.printStackTrace();
		}
		
	}

```
