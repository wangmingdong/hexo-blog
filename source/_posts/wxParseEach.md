---
title: wxParse多数据循环使用方法
date: 2018-11-07 16:15:31
tags: ['微信小程序', 'wxParse']
categories: ['微信小程序']
---

介绍如何使用wxParse在列表等多条HTML共同渲染的方法
--------------

## 第一种（官方方法）

### 方法介绍
``` js
/**
* WxParse.wxParseTemArray(temArrayName,bindNameReg,total,that)
* 1.temArrayName: 为你调用时的数组名称
* 3.bindNameReg为循环的共同体 如绑定为reply1，reply2...则bindNameReg = 'reply'
* 3.total为reply的个数
*/
var that = this;
WxParse.wxParseTemArray("replyTemArray",'reply', replyArr.length, that)
```

### 使用方式
* 循环绑定数据

``` js
var replyHtml0 = `<div style="margin-top:10px;height:50px;">
    <p class="reply">
        wxParse回复0:不错，喜欢[03][04]
    </p>	
</div>`;
var replyHtml1 = `<div style="margin-top:10px;height:50px;">
    <p class="reply">
        wxParse回复1:不错，喜欢[03][04]
    </p>	
</div>`;
var replyHtml2 = `<div style="margin-top:10px;height:50px;">
    <p class="reply">
        wxParse回复2:不错，喜欢[05][07]
    </p>	
</div>`;
var replyHtml3 = `<div style="margin-top:10px;height:50px;">
    <p class="reply">
        wxParse回复3:不错，喜欢[06][08]
    </p>	
</div>`;
var replyHtml4 = `<div style="margin-top:10px; height:50px;">
    <p class="reply">
        wxParse回复4:不错，喜欢[09][08]
    </p>	
</div>`;
var replyHtml5 = `<div style="margin-top:10px;height:50px;">
    <p class="reply">
        wxParse回复5:不错，喜欢[07][08]
    </p>	
</div>`;
var replyArr = [];
replyArr.push(replyHtml0);
replyArr.push(replyHtml1);
replyArr.push(replyHtml2);
replyArr.push(replyHtml3);
replyArr.push(replyHtml4);
replyArr.push(replyHtml5);


for (let i = 0; i < replyArr.length; i++) {
    WxParse.wxParse('reply' + i, 'html', replyArr[i], that);
    if (i === replyArr.length - 1) {
    WxParse.wxParseTemArray("replyTemArray",'reply', replyArr.length, that)
    }
}
```
* 模版使用

``` html
<block wx:for="{{replyTemArray}}" wx:key="">
    回复{{index}}:<template is="wxParse" data="{{wxParseData:item}}"/>
</block>
```

## 第二种 为每一个节点设置id标识

### 使用方式

* 循环绑定id

``` js
newsItems.forEach(function (v, i) {
    // 对于返回的html片段做处理
    WxParse.wxParse('article' + i, 'html', v.contents, self, 5);
}); 
```
* 模板使用

``` html
<view class="sp-new" wx:key="gid" wx:for="{{newsData}}">
    <view class="sp-new-title">
    {{item.title}}
    </view>
    <template is="wxParse" data="{{wxParseData : self['article'+index].nodes }}"/>
</view>
```
