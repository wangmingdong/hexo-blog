---
title:  微信小程序架构实现
date: 2018-12-11 16:04:52
tags: ['小程序', '架构']
categories: ['小程序']
---
## 概述

作为前端开发，只是会使用而不动框架原理会踩很多坑，难以找到原因。本文从小程序原理讲讲底层是如何实现的。

## 小程序目录

**一个完整的小程序主要由以下几部分组成：** 
* 一个入口文件：app.js 
* 一个全局样式：app.wxss 
* 一个全局配置：app.json 
* 页面：pages下，每个页面再按文件夹划分，每个页面4个文件 
* 视图：wxml，wxss 
* 逻辑：js，json（页面配置，不是必须）

注：pages里面还可以再根据模块划分子目录，孙子目录，只需要在app.json里注册时填写路径就行。

## 打包
打包的实现就涉及到这个编辑器的实现原理和方式了，它本身也是基于WEB技术体系实现的，==nwjs+react==，nwjs是什么：简单是说就是node+webkit，node提供给我们本地api能力，而webkit提供给我们web能力，两者结合就能让我们使用JS+HTML实现本地应用程序。 
既然有nodejs，那上面的打包选项里的功能就好实现了。 
ES6转ES5：引入babel-core的node包 
CSS补全：引入postcss和autoprefixer的node包（postcss和autoprefixer的原理看这里） 
代码压缩：引入uglifyjs的node包

注：在android上使用的x5内核，对ES6的支持不好，要兼容的话，要么使用ES5的语法或者引入babel-polyfill兼容库。

## 打包后的目录
![小程序打包后目录](https://raw.githubusercontent.com/wangmingdong/docImg/master/2018-12-11-wxchat-how-to-work/20170326214333007.jpeg)
所有的小程序基本都最后都被打成上面的结构 
* WAService.js 框架JS库，提供逻辑层基础的API能力 
* WAWebview.js 框架JS库，提供视图层基础的API能力 
* WAConsole.js 框架JS库，控制台 
* app-config.js 小程序完整的配置，包含我们通过app.json里的所有配置，综合了默认配置型 
* app-service.js 我们自己的JS代码，全部打包到这个文件 
* page-frame.html 小程序视图的模板文件，所有的页面都使用此加载渲染，且所有的WXML都拆解为JS实现打包到这里 
* pages 所有的页面，这个不是我们之前的wxml文件了，主要是处理WXSS转换，使用js插入到header区域。

## 小程序架构
微信小程序的框架包含两部分View视图层、App Service逻辑层，View层用来渲染页面结构，AppService层用来逻辑处理、数据请求、接口调用，它们在两个进程（两个Webview）里运行。 
视图层和逻辑层通过系统层的JSBridage进行通信，逻辑层把数据变化通知到视图层，触发视图层页面更新，视图层把触发的事件通知到逻辑层进行业务处理。

![小程序架构](https://raw.githubusercontent.com/wangmingdong/docImg/master/2018-12-11-wxchat-how-to-work/20170326215724891.jpeg)

小程序启动时会从CDN下载小程序的完整包，一般是数字命名的,如：_-2082693788_4.wxapkg

## 小程序技术实现
小程序的UI视图和逻辑处理是用多个webview实现的，逻辑处理的JS代码全部加载到一个Webview里面，称之为AppService，整个小程序只有一个，并且整个生命周期常驻内存，而所有的视图（wxml和wxss）都是单独的Webview来承载，称之为AppView。所以一个小程序打开至少就会有2个webview进程，正式因为每个视图都是一个独立的webview进程，考虑到性能消耗，小程序不允许打开超过5个层级的页面，当然同是也是为了体验更好。

## AppService
可以理解AppService即一个简单的页面，主要功能是负责逻辑处理部分的执行，底层提供一个WAService.js的文件来提供各种api接口，主要是以下几个部分： 
消息通信封装为WeixinJSBridge（开发环境为window.postMessage, IOS下为WKWebview的window.webkit.messageHandlers.invokeHandler.postMessage，android下用WeixinJSCore.invokeHandler）

 * 日志组件Reporter封装 
 * wx对象下面的api方法 
 * 全局的App,Page,getApp,getCurrentPages等全局方法 
 * 还有就是对AMD模块规范的实现

然后整个页面就是加载一堆JS文件，包括小程序配置config，上面的WAService.js（调试模式下有asdebug.js），剩下就是我们自己写的全部的js文件，一次性都加载。

## 在开发环境下

 * 页面模板：app.nw/app/dist/weapp/tpl/appserviceTpl.js 
 * 配置信息，是直接写入一个js变量，__wxConfig。 
 * 其他配置
 
![其他配置](https://raw.githubusercontent.com/wangmingdong/docImg/master/2018-12-11-wxchat-how-to-work/20170326220419423.jpeg)

## 线上环境
而在上线后是应用部分会打包为2个文件，名称app-config.json和app-service.js，然后微信会打开webview去加载。线上部分应该是微信自身提供了相应的模板文件，在压缩包里没有找到。 
1. WAService.js（底层支持） 
2. app-config.json（应用配置） 
3. app-service.js（应用逻辑）

然后运行在JavaScriptCore引擎里面。

## AppView
这里可以理解为h5的页面，提供UI渲染，底层提供一个WAWebview.js来提供底层的功能,具体如下： 
1. 消息通信封装为WeixinJSBridge（开发环境为window.postMessage, IOS下为WKWebview的window.webkit.messageHandlers.invokeHandler.postMessage，android下用WeixinJSCore.invokeHandler） 
2. 日志组件Reporter封装 
3. wx对象下的api，这里的api跟WAService里的还不太一样，有几个跟那边功能差不多，但是大部分都是处理UI显示相关的方法 
4. 小程序组件实现和注册 
5. VirtualDOM，Diff和Render UI实现 
6. 页面事件触发

在此基础上，AppView有一个html模板文件，通过这个模板文件加载具体的页面，这个模板主要就一个方法，$gwx，主要是返回指定page的VirtualDOM，而在打包的时候，会事先把所有页面的WXML转换为ViirtualDOM放到模板文件里，而微信自己写了2个工具wcc（把WXML转换为VirtualDOM）和wcsc（把WXSS转换为一个JS字符串的形式通过style标签append到header里）。
