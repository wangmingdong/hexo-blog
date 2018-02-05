---
title: Centos下安装Nodejs
date: 2018-02-05 17:45:45
tags: ["node", 'linux', 'centos']
categories: ['linux']
---
### 包管理安装

百度一堆乱七八糟的教程，加上node各种版本，其实官方稳定就有包管理的安装方法，几条命令就可以安装好各种版本。
官方文档地址：<https://nodejs.org/en/download/package-manager/>

Run as root on RHEL, CentOS or Fedora, for Node.js v4 LTS Argon:

``` bash
curl --silent --location https://rpm.nodesource.com/setup_4.x | bash -
```

Alternatively for Node.js v6:

``` bash
curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
```

Alternatively for Node.js 0.10:

``` bash
curl --silent --location https://rpm.nodesource.com/setup | bash -
```

Then install, as root:

``` bash
yum -y install nodejs
```

就这样，不能再简单。

### 源码安装

来自官方文档（会看官方文档多重要）
<https://github.com/nodejs/node/wiki/Installation>
准备命令：
``` bash
yum -y install gcc make gcc-c++ openssl-devel wget
```

下面是安装版本0.12.7，安装其他版本只需要替换掉就行，所有版本地址：<https://nodejs.org/dist/>

``` bash
ver=0.12.7 #Replace this with the latest version available
wget -c https://nodejs.org/dist/v$ver/node-v$ver.tar.gz #This is to download the source code.
tar -xzf node-v$ver.tar.gz
cd node-v$ver
./configure && make && sudo make install
```

验证是否安装配置成功：

``` bash
node -v
npm -v
```