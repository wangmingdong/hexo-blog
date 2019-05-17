---
title: SmartGit过期后破解方法
date: 2019-05-17 11:06:04
tags: ['git', 'smartGit']
categories: ['git']
---

* 根据自己的操作系统，进入相应的文件夹 ，可能还有一个版本号的文件夹，再进入

``` bash
Windows： %APPDATA%\syntevo\SmartGit\
OS X： ~/Library/Preferences/SmartGit/
Unix/Linux： ~/.smartgit/
```
* 删除settings.xml这个文件，比如mac下文件在

``` bash
~/Library/Preferences/SmartGit/8/settings.xml
```

* 重新进入SmartGit，正常。