---
title: Mac OS X 安装Ruby
date: 2019-01-04 15:02:18
tags: ['mac', 'ruby']
categories: ['工具']
---

<b>安装CocoaPods第一步</b>
起因:重装系统后需要重新安装CocoaPods网上搜了下发现很多都过时了，已经不能用了。而且taobao Gems源已经停止服务，现在有ruby-china提供服务
<b>PS："$"开头表示需要在终端下执行</b>

## 1.安装RVM

``` bash
$ curl -L https://get.rvm.io | bash -s stable
```
<b>期间可能需要输入密码(我安装时没有提示，密码就是开机密码输入时密码不会显示直接输入完成就可以)，等待一段时间将安装好(大概五六分钟)。</b>

![img1](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-01-04-mac-install-ruby/1.png)

## 2.载入RVM环境
若打开新终端窗口则不用执行
``` bash
$ source ~/.rvm/scripts/rvm
```

![img2](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-01-04-mac-install-ruby/2.png)

## 3.检查RVM是否安装好

``` bash
$ rvm -v
```

![img3](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-01-04-mac-install-ruby/3.png)

## 4.安装Ruby

##### 1>列出已知的ruby版本

``` bash
$ rvm list known
```

![img4](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-01-04-mac-install-ruby/4.png)

##### 2>选择2.0.0版本进行安装(其他版本也可以)

<b>等待下载(途中需要按回车确定安装路径、还要输入密码)、编译。完成之后Ruby、Ruby Gems就安装好了</b>

``` bash
$ rvm install 2.0.0
```

![img5](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-01-04-mac-install-ruby/5.png)

##### 3>查询已安装的ruby

``` bash
$ rvm list
```

![img6](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-01-04-mac-install-ruby/6.png)

##### 4>卸载已安装的版本(若已经安装过ruby）

``` bash
$ rvm remove [版本号]
```

## 4.设置Ruby版本

``` bash
$ rvm 2.0.0 —default
```

![img7](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-01-04-mac-install-ruby/7.png)

检查是否安装好了

``` bash
$ rvm -v
```

![img8](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-01-04-mac-install-ruby/8.png)

``` bash
$ gem -v
```

![img9](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-01-04-mac-install-ruby/9.png)

## 5.更换Ruby源
我们需要来修改更换Ruby源，国内镜像源 taobao 源 已经停止维护了（由于国内被墙）所以要把源切换至ruby-china

##### 1>检测Ruby源
``` bash
$ gem sources -l
```
检查结果：（ 如果电脑没安装过 CocoaPods，此时应该是默认 ruby 源 ）

``` bash
huanghaipoMacBook-Pro:~ jijiucheng$ gem sources -l *** CURRENT SOURCES *** https://rubygems.org/
```

##### 2>移除 ruby 源

``` bash
$ gem sources --remove https://rubygems.org/
```

##### 3>移除结果：

``` bash
huanghaipoMacBook-Pro:local jijiucheng$ gem sources --remove https://rubygems.org/
https://rubygems.org/ removed from sources
```

替换添加国内镜像源 ruby-china 源，因为上面已经提到国内镜像源 taobao 源 已经停止维护了，所以此处替换的是 ruby-china 源，且尽量确保只有一个 ruby-china 源

``` bash
$ gem sources --add https://gems.ruby-china.org
```

##### 4>替换结果：

``` bash
huanghaipoMacBook-Pro:local jijiucheng$ gem sources --add https://gems.ruby-china.org
https://gems.ruby-china.org added to sources
```

##### 5>再次检查此时的 ruby 源：（ 已经变成了 ruby-china 源 ）

``` bash
huanghaipoMacBook-Pro:local jijiucheng$ gem sources -l
*** CURRENT SOURCES ***
https://gems.ruby-china.org
```

