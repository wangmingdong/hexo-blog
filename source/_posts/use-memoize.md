---
title: 使用Memoize提高递归效率
date: 2018-11-08 17:26:18
tags: ['javascript', '递归']
categories: ['javascript']
---


## 什么是Memoize

* Memoize是一种利用缓存解决递归（函数）重复计算，提高效率的方式，比如阶乘、菲薄那次数；
* 对于第一次的计算并没有提高效率，只有重复计算才会利用缓存取值；
* 使用场景大部分为递归

## Fibonacci例子

实现Fibonacci输出第N个数，代码如下：
``` js
function fibonacci(n){
   if(n==0||n==1){
        return n;
    }
    return fibonacci(n-1) + fibonacci(n-1);
}
```
比如计算fibonacci(4)，首先得计算fibonacci(3)和fibonacci(2)，而要计算fibonacci(3)需要先计算fibonacci(2)和fibonacci(1)......像树一样每个分之都需要重复计算，直到计算出fibonacci(1)。

能否有种方式可以把fibonacci(1)fibonacci(2)fibonacci(3)......第一次计算的值存储起来，这样后面的递归直接取值？

这就诞生出了Memoize

## Memoize
一个通用的Memoize方法如下：
``` js
/**
 * JavaScript Momoization
 * @param {string} func name of function / method
 * @param {object} [obj] mothed's object or scope correction object
 *
 * MIT / BSD license
 */
 
function Memoize(func, obj){
    var obj = obj || window,
        func = obj[func],
        cache = {};
    return function(){
        var key = Array.prototype.join.call(arguments, '_');
        if (!(key in cache))
            cache[key] = func.apply(obj, arguments);
        return cache[key];
    }
}
```

## 利用Memoize改写Fibonacci方法
``` js
var fibonacci = (function(){
  var cache = [];　　　　　　　　　　 //定义一个空的存放缓存的数组
  return function(n){　　　　　　
    if(n === 0 || n === 1){
      return n;
    }else{
      cache[n-1] = cache[n-1]||fibonacci(n-1); //先从cache数组里查询结果，如果没找到的话在计算
      cache[n-2] = cache[n-2]||fibonacci(n-2);
      return cache[n-1]+cache[n-2];
    }
  }
})();
```

一次项目写树型结构时候用到很多重复递归，由于得正向递归和逆向递归，界面会出现2秒空白，如今学习到Memoize，得空重构下代码应该会提高不少效率