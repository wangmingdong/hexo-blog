---
title: 监听微信浏览器页面被挂起状态
date: 2019-08-15 16:03:58
tags: ["微信", '公众号开发']
categories: ['js']
---

前阵子做公众号开发，需要处理一个 ios 上按 HOME 切回桌面，微信页面被挂起的相关bug，在此记录一下：

h5有一个事件 ``visibilitychange``可以用于监听当前页面是否被挂起，如下：

``` js
document.addEventListener('visibilitychange', () => { 
    if(document.hidden) {
        // 页面被挂起
    }
    else {
        // 页面呼出
    }
})
```
但是，手q不会触发此事件！因为手q网页永远不会被挂起，所以``visibilitychange``事件不会被触发。

可以在手q中运行如下代码：

``` js
setInterval(() => document.write(new Date().getTime() + "\<br\>"), 1000);
```
ios按下HOME键等待十几秒后呼起手q，你会发现在页面输出的时间并没有中断，而是很平稳地输出。

为了解决这一问题，手q提供了一个叫``qbrowserVisibilityChange``的事件：
[https://open.mobile.qq.com/api/common/index#api:qbrowserVisibilityChange](https://open.mobile.qq.com/api/common/index#api:qbrowserVisibilityChange)

开发者可以通过此事件监听手q是否被挂起（注意我这里说的是手q是否被挂，而不是手q的网页被挂起）。如果按照腾讯提供的api，那么调用``qbrowserVisibilityChange``需要引入一个js，如下：

``` html
<script type="text/javascript" src="http://pub.idqqimg.com/qqmobile/qqapi.js?_bid=152&quot"></script>;
```

它的使用方式如下：

``` js
mqq.addEventListener("qbrowserVisibilityChange", function(e){
    if(e.hidden) {
        // 手Q被挂起
    }
    else {
        // 手Q被呼起
    }
});
```
后来发现，其实手q把 ``qbrowserVisibilityChange`` 直接绑定到document对象上。也就是说手q现在不需要引用额外的js文件就可以直接调用 ``qbrowserVisibilityChange`` 事件，如下：

``` js
document.addEventListener("qbrowserVisibilityChange", function(e){
    if(e.hidden) {
        // 手Q被挂起
    }
    else {
        // 手Q被呼起
    }
});
```

目前来说，微信是支持 ``visibilitychange`` 事件的，这也就是说微信被挂起时，微信的内嵌页面也会被挂起。

不过此处需要注意的是： **如果微信的``webview``是``UIWebView``的话，那么不支持``visibilitychange``**，也没有一个对应事件可以监听微信是否被挂起，一句话误解。好消息是UIWebView只存在于2017年前的版本中，即 UIWebView的情况可以被忽略。

可以使用以下方法来监听 微信手q 的挂起状态：

``` js
document.addEventListener("qbrowserVisibilityChange", function(e){
    var evt = document.createEvent("HTMLEvents"); 
    evt.initEvent("visibilitychange", false, false); 
    evt.hidden = e.hidden; 
    document.dispatchEvent(evt); 
}, true); 
document.addEventListener("visibilitychange", function(e) {
    e.hidden = e.hidden === undefined ? document.hidden : e.hidden; 
}, true); 
```

在业务代码直接引用如下的代码：
``` js
document.addEventListener("visibilitychange", function(e) {
  text.innerHTML = e.hidden; 
    if(e.hidden) {
        // 网页被挂起
    }
    else {
        // 网页被呼起
    }
});
```
**挂起事件有什么用吗？**

如果微信手Q的网页有音频播放，那么按下 HOME 键挂起应用后，音频也会继续在播放。这个时候使用 visibilitychange 事件就有用了。如下：

``` js
document.addEventListener("visibilitychange", function(e) {
    if(e.hidden) {
        // 网页被挂起 ---- 暂停音乐
        audio.pause(); 
    }
    else {
        // 网页被呼起 ---- 播放音乐
        audio.play(); 
    }
}); 
```