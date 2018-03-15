---
title: 解决vue项目上使用hexo请求变为request payload
date: 2018-03-15 14:30:15
tags: ['vue', 'hexo']
categories: ['vue']
---

### 引子
前段时间在使用vue搭建项目中碰到一个坑，我用自定义配置方式引入axios，此时post请求莫名其妙的变为Requst Payload，而我们常用的是Form Data，Requst Payload请求服务无法获取param，此时就要想办法如何转化数据了。

### 尝试
度娘上搜索，发现不少人说修改头信息为：
``` js
headers: {
    'Content-Type': 'application/x-www-form-urlencoded;application/json;charset=UTF-8;'
  },
```
我尝试后，发现类型真的变为form data，但是参数转化有问题：
``` js
{"loginNo":"test","loginPwd":"testtest"}:
```
并不是我们想要的结果，而且结尾处总是有":"

### qs
经查询发现qs库，虽然有了JSON.stringify方法，但和qs转化结果并不一样
比如有数据 
``` js
var a = {name:'hehe',age:10};
```

使用JSON.stringify后为
```js
"{"a":"hehe","age":10}"
```

而使用qs.stringify后：
```js
name=hehe&age=10
```

### 最终尝试
axios官方文档上 Request Config 提供了一个配置：
``` js
transformRequest: [function (data, headers) {
  // Do whatever you want to transform the data

  return data;
}]
```
那我们把需要的参数放在这里转化一下即可
``` js
var qs = require('qs')
export function login(username, password) {
  return request({
    url: '/login',
    method: 'post',
    transformRequest: [function(data) {
      return qs.stringify({ param: JSON.stringify({ loginNo: username, loginPwd: password }) })
    }]
  })
}
```
控制台：
``` js
Form Data

param:{"loginNo":"test","loginPwd":"testtest"}
```
可以愉快的跟服务进行post请求了
