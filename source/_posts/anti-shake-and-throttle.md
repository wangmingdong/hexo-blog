---
title: 防抖函数和节流函数
date: 2019-09-17 11:26:01
tags: ["js算法", '代码优化']
categories: ['js']
---

## 一、防抖函数

#### 1.1 概念：

触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间。

#### 1.2 使用场景：

就像是我的搜索栏功能，是在里面内容变化后就实时触发搜索事件，但是有时候我们输的内容很长，在没有输完的时候就触发了事件，这样对性能来说是不好的，造成了很多无用的请求，这时候就需要用到防抖函数，来让其在搜索内容变化后的200毫秒内如果没有再更改才发起请求。

#### 1.3 实现防抖函数的思路：

在高频触发事件的时候，取消原来的延时事件。

#### 1.4 具体实现：

``` js
function debounce( fn ){ // 传一个回调函数
    let Mytime = null ;
    return function( ){ 
        clearTimeout( Mytime ) ; // 清除定时器
        Mytime  = setTimeout( () => {
             fn.apply(this,arguments)
         }, 200)
    }
}
```

#### 1.5 具体使用
``` js
function sayHi() {
    console.log('防抖成功');
}

var select = document.getElementById('myselect');
select .addEventListener('change', debounce(sayHi)); // 防抖
```

## 二、节流函数

#### 2.1 概念：

　　高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率；

#### 2.2 使用场景：

　　就像我接了一个任务，只能在5秒完成任务给回复，在执行后的这5秒内，你再怎么给我布置任务我都不管直接当没听到，直到到5秒后执行完这个任务，你才可以再次给我布置任务，以此类推。。。

#### 2.3 实现思路：

　　每次触发事件时都判断当前是否有等待执行的延时函数，如果有直接截断事件不用往下执行了；

#### 2.4 具体实现：

``` js
function throttle( fn ){
    let canUse = true ; // 设置一个开关
    return function(){
        if(!canUse ){ return false } // 如果开关已经关掉了就不用往下了
        canUse  = false  // 利用闭包刚进来的时候关闭开关
        setTimeout( ( ) => { 
            fn.apply(this,arguments)
　　　　　　　　　　canUse = true // 执行完才打开开关
        },500)
    }
}
```

#### 2.5 具体使用：
``` js
function sayHi(e) {
    console.log(e.target.innerWidth, e.target.innerHeight);
}
window.addEventListener('resize', throttle(sayHi));
```

## 三、个人理解两者的区别

防抖函数和节流函数的一个区别就是防抖是按照最后一次触发为下一次事件执行的时间计算点，
前面的没满足间隔时间的都从最后这一次开始来决定什么时候执行延时事件；
而节流函数是按照第一次触发事件作为时间计算点，后面没到间隔时间的事件都忽略；