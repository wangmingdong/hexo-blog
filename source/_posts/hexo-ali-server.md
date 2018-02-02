---
title: 如何把hexo博客部署在阿里云服务器上
date: 2018-02-02 15:22:27
tags: ['hexo', '阿里云服务器']
categories: ['hexo']
---

## 配置Hexo
基础配置就不多说了，主要说下配置文件和deploy，如何用ftp方式把网站推送到虚拟主机上

## 更新文件

新版hexo取消了默认安装ftpsync， 会报错 ERROR Deployer not found: ftpsync 手动安装即可

``` bash
$ npm install hexo-deployer-ftpsync --save
```

``` bash
deploy:
  type: ftpsync
  host: bxu******.my3w.com //主机地址
  user: bxu******          //用户名
  pass: *********          //密码
  remote: /htdocs          //目录，应该所有阿里云虚拟主机的网站内容目录都是这个，不是根目录 /
  port: 
  ignore: .DS_Store
  connections: 
  verbose: true
```

## 生成发布文件

``` bash
$ hexo generate
```
或者
``` bash
$ hexo g
```
此时如果你要手动部署（那我们还配置deploy搞毛用）， 使用你的FTP工具直接将 public 下的文件放进 虚拟主机的 htdocs 目录下，刷新你的网站，就看到效果了

## 一键发布

``` bash
$ hexo deploy
```

刷新网站， 就可以看到你写的东西了

