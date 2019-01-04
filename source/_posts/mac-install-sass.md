---
title: sass安装更新及卸载方法
date: 2019-01-04 15:22:52
tags: ['mac', 'ruby', 'sass']
categories: ['工具']
---

* 在 Windows/Mac 平台下安装 Ruby 需要先有 Ruby 安装包，大家可以到 Ruby 的官网（http://rubyinstaller.org/downloads）下载对应需要的 Ruby 版本。
  
* Ruby 安装文件下载好后，可以按应用软件安装步骤进行安装 Ruby。如果是windows，在安装过程中，个人建议将其安装在 C 盘下，一直按下一步就好，最后安装完成后会出现一个类似命令行工具的界面。
  
* 当你的电脑中安装好 Ruby 之后，接下来就可以安装 Sass 了。同样的在windows/Mac下安装 Sass 有多种方法。但这几种方法都是非常的简单，只需要在你的命令终端输入一行命令即可。

## 1.通过命令安装 Sass：

打开电脑的命令终端，输入下面的命令：

``` bash
$ gem install sass
```

提醒一下，在使用 Mac 的同学，可能需要在上面的命令前加上"sudo"，才能正常安装：

``` bash
$ sudo gem install sass
```

如果上面的方法没有安装成功，可以使用下面的两种方法。

## 2.通过 Compass 来安装 Sass

除了使用 gem 命令来安装 Sass 之外，还可以通过安装 compass来安装 Sass，因为 Compass 是基于 Sass 开发的一个框架。也就是说，你安装了 Compass，也就同时安装好了 Sass。

同样的在你的命令终端输入下面的命令：

``` bash
$ sudo gem install sass
```

执行完上面的命令之后，就开始安装 Compass 和 Sass。

注：Compass 是一个成熟的、基于 Sass 开发的一个框架，这里面集成了很多写好的 mixins 和 Sass 函数。不过在此暂不做过多阐述。

## 3.本地安装 Sass

由于有时候直接使用上面的命令安装会让你无法正常实现安装（网络受限原因），当碰到这种情况之时，那么安装需要特殊去处理，可以通过下面的方法来实现 Sass 的正常安装：

可以到 Rubygems(http://rubygems.org/) 网站上将 Sass 的安装包（http://rubygems.org/gems/sass）下载下来，然后在命令终端输入：

``` bash
$ gem install <把下载的安装包拖到这里>
```

直接回车即可安装成功。

注：在 iOSX 系统平台，可以直接将下载的安装包拖到 "gem install" 后面，如果在是 Windows 系统，需要手功输入安装的文件路径。

## 4.淘宝 RubyGems 镜像安装 Sass

除了下载 Sass 安装包到本地安装之外，碰到网络原因无法安装时还可以使用淘宝 RubyGems 镜像安装 Sass。只是我们需要通过 gem sources 命令来配置源，先移除默认的 https://rubygems.org 源，然后添加淘宝的源 https://ruby.taobao.org：

第一步：移动默认的源

``` bash
$ gem sources --remove https://rubygems.org/
```

第二步：指定淘宝的源

``` bash
$ gem sources -a https://ruby.taobao.org/
```

第三步：查看指定的源是不是淘宝源

``` bash
$ gem sources -l
```

返回结果如下：

``` bash
*** CURRENT SOURCES ***
https://ruby.taobao.org
```

请确保只有 ruby.taobao.org。如果无误之后，执行下面的命令：

``` bash
$ gem install sass
```

等待安装完毕。如何检测安装成功呢?输入以下命令：

``` bash
$ sass -v
```

如果在你的命令终端能看到类似这样的信息就表示你的电脑安装 Sass 已成功。也就是说可以正常的使用 Sass 了。

``` bash
> sass -v
Ruby Sass 3.5.7
```

## 5.更新 Sass

维护 Sass 的团队会不断的为 Sass 添加新的功能，那么如何确保自己已安装的 Sass 也具有这些新的功能特性呢？不会是卸载了重新安装吧（虽然安装也就是一个命令的事情）？ 其实不需要这么麻烦，只需要在命令终端执行：

``` bash
$ gem update sass
```

这个时候你看到类似下面这样的信息，表示你的 Sass 已更新到最新版本。

``` bash
> gem update sass
Updating installed gems
Nothing to update
```

## 卸载（删除）Sass

在常期使用的时候难免会碰到无法解决的问题，有时候可能需要卸载 Sass，然后再重新安装 Sass。那么怎么卸载 Sass 呢？

其实他也就是一句命令的事情：

``` bash
$ gem uninstall sass
```
