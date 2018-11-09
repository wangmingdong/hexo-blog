---
title: 浅谈继承的实现
date: 2018-11-09 15:34:24
tags: ['javascript', 'prototype']
categories: ['javascript']
---

## 前言

想起曾经被面试和面试的经历，说道js三大特性时候，多态可能比较抽象，但继承应该是面向对象编程的基本，项目里应该经常用到。如今ES6实现更为方便，但为了更好地理解，先看看ES5的实现方式。

（之前写过`prototype`和`__proto__`，这里不做细说，主要说说如何实现继承）

---

比如这样一个场景：对象为学生，学生有名字和年龄，可以吃和说话

## ES5实现继承

``` js
// 首先定义Student构造函数，声明学生应有的属性
function Student (student) {
    this.name = student.name;
    this.age = student.age;
}
// 学生的行为，如eat，talk，依然使用prototype为构造函数实现继承
Student.prototype.eat = function () {
    console.log('eat');
}
Student.prototype.talk = function () {
    console.log('talk');
}
// new关键字，开辟新的地址，此时stu1和stu2除了有name和age属性，还继承了eat和talk方法
var stu1 = new Student({
    name: '张三',
    age: 6
})
var stu2 = new Student({
    name: '李四',
    age: 8
})
console.log(stu1.name)  // 张三
console.log(stu2.eat()) // eat
console.log(stu1 instanceof Student)    // true stu1和stu2都继承于Student
console.log(stu1 == stu2)   // false 由于new所以占用的不同的地址
```
这里比较关键的是 new 这个关键字，它实际上做了`四`件事：

* `new Object()` 创建一个空对象
* 将构造函数（`Student`）的作用域赋给新对象（因此 `this` 就指向了这个新对象）
* 执行构造函数中的代码（为这个新对象添加属性，这里赋予了name、age、eat和talk）
* 返回一个新对象

## ES6实现继承

ES6提供新的关键字 `class`

``` js
//定义类
class Student {
  constructor(student) {
    this.name = student.name;
    this.age = student.age;
  }

  eat() {
    console.log('eat');
  }

  talk() {
      console.log('talk');
  }
}

let stu3 = new Student({
    name: '王五',
    age: 7
})
let stu4 = new Student({
    name: '赵六',
    age: 5
})
console.log(stu3.name)  // 王五
console.log(stu4.talk()) // talk
console.log(stu3 instanceof Student)    // true stu3和stu4都继承于Student
console.log(stu3 == stu4)   // false 由于new所以占用的不同的地址
```
## 好处

继承是面向对象编程的一种实现方式，可以将构造对象和业务处理分离，简单来说，我们在一个模块对照服务返回（数据库表）字段构造对象，完成后就可以专心的在业务模块写业务，代码清晰更方便维护。

对于复杂大型项目，每个业务文件里都要重新定义生成对象是一件非常繁琐又冗余的工作。
