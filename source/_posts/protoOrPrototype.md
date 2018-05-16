---
title: __proto__和prototype的关系
date: 2018-05-16 22:01:42
tags: ["__proto__", 'prototype', 'constructor']
categories: ['js']
---

### 一、构造函数

``` js
function Foo(){};
var f = new Foo;
```
用来初始化新建对象的函数称为构造函数，上述例子中Foo就是构造函数。

``` js
var f1 = new Foo;
var f2 = new Foo;
console.log(f1 === f2);    // false
```
每new出来一个新对象都会为它开辟一个新的地址空间，所以f1不是绝对等于f2。

### 二、__proto__和prototype
首先要知道的是，__proto__和prototype不是一个概念。

__proto__称之为隐式原型，实例对象的__proto__指向的是构造该对象的构造函数的原型。
``` js
function Foo(){};
var f1 = new Foo;
console.log(f1.__proto__ === Foo.prototype);    //true
```
需要了解的是__proto__是非标准得到对象的原型对象的方法，Chrome和Firfox对__proto__支持比较好，但是IE就不行了。标准获取方式是Object.getPropertyOf(obj)，这是ES5提供获取对象的原型对象的标准方法。

而prototype为显示原型，指向实例对象的原型对象。通过同一个构造函数实例化出多个对象，这些对象具有相同的原型对象。

__proto__是每个对象都具有的属性，而prototype是构造函数才具有的。

举个例子：
``` js
function Foo(){};
Foo.prototype.a = 1;
var f1 = new Foo;
var f2 = new Foo;

console.log(f1.a == f2.a);    // true
console.log(f1.__proto__ === f2.__proto__);    //true
```
下面是我画的一张简单的__proto__和prototype对应图：
![__proto__和prototype对应图](https://raw.githubusercontent.com/wangmingdong/docImg/master/protoAndPrototype.png)

我们通过prototype实现JS的继承。

### 三、constructor
原型对象有一个属性叫constructor，指向该原型对象对应的构造函数；
``` js
function Foo(){};
console.log(Foo.prototype.constructor === Foo);    // true
```
由于继承的关系，Foo实例出的对象也具有constructor属性，指向原型对象对应的构造函数：

``` js
var f = new Foo();
console.log(f.constructor === Foo);    // true
```