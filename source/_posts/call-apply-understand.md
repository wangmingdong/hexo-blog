---
title: 深入理解call和apply
date: 2018-02-02 17:26:08
tags: ['javascript', 'call', 'apply']
categories: ['javascript']
---
### 定义
call和apply的作用是一样的，都是为了改变函数体内部this的指向，不同的是接受的参数不同；

### 不同
除了第一个参数对象相同，后面的参数不太一样，call需要的是分别列举出的对象，apply需要的则是数组；
``` js
function add(c, d){
  return this.a + this.b + c + d;
}

var s = {a:1, b:2};
console.log(add.call(s, 3, 4)); // 1+2+3+4 = 10
console.log(add.apply(s, [5, 6])); // 1+2+5+6 = 14 
```

### 理解call或apply含义
举个例子
``` js
var cat = {
  food: 'fish',
  say: function () {
    console.log('I eat ' + this.food);
  }
}

```
来了只狗，狗要吃骨头，但是不想重新定义say，此时：
``` js
var dog  = {
  food: 'bone'
};
cat.say.call(dog) // I eat bone
```

还是不能理解么？知乎上看到了一段话，觉得描述很形象：

``` html
猫吃鱼，狗吃肉，奥特曼打小怪兽。

有天狗想吃鱼了

猫.吃鱼.call(狗，鱼)

狗就吃到鱼了

猫成精了，想打怪兽

奥特曼.打小怪兽.call(猫，小怪兽)
```