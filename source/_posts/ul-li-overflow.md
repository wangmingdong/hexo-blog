---
title: '解决ul li 无法出现横向滚动条问题'
date: 2018-05-30 17:51:44
tags: ["css", 'overflow']
categories: ['css']
---
由于li设置的float:left，导致ul设置overflow-x:scoll无效，解决办法是：
``` css
ul:{
    display: -webkit-box;
    display: -webkit-flex;
    display: -moz-box;
    display: -moz-flex;
    display: -ms-flexbox;
    display: flex;
}

li:{
    float: none;
}
```
此时有个问题，用display:flex时li中的文字排版会出现换行，此时设置li white-space: nowap便可。