---
title: 如何下载和安装MongoDB
date: 2018-02-06 16:42:31
tags: ['MongoDB']
categories: ['数据库']
---

### 下载

官网： <http://www.mongodb.org/>
下载比较麻烦，可以直接从<http://dl.mongodb.org/dl/win32/x86_64>下载msi或者zip

### 安装

我下载的msi，最好选custom自定义，不然找不到安装位置，比如我安装到了C盘根目录；
__安装好后在根目录新建data文件夹（用于存储数据，这步很重要，不然会闪退）__，
打开cmd命令窗口，进入到mongodb\bin目录下输入：

``` bash
mongod --dbpath c:/data
# c:/data 为你data路径
```

执行后：

``` bash
...
2018-02-06T16:40:27.441+0800 I INDEX    [initandlisten]          building index using bulk method; build may temporarily use up to 500 megabytes of RAM
2018-02-06T16:40:27.453+0800 I INDEX    [initandlisten] build index done.  scanned 0 total records. 0 secs
2018-02-06T16:40:27.455+0800 I COMMAND  [initandlisten] setting featureCompatibilityVersion to 3.4
2018-02-06T16:40:27.456+0800 I NETWORK  [thread1] waiting for connections on port 27017
```
如果末尾显示： waiting for connections on port 27017
证明安装成功。

### 测试
连接成功以后就可以去浏览器打开<http://localhost:27017>
如果看到

It looks like you are trying to access MongoDB over HTTP on the native driver port.

则已经连接成功了。



