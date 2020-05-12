---
title: 括号匹配算法
date: 2020-05-12 15:50:47
tags: ["js算法"]
categories: ['js']
---

想起曾经被问过一个与栈有关的问题，刚好有空整理下思路。

问题：给定一个字符串，判断该字符串是否满足括号匹配，如：[{()}[]]  匹配；[{(}] 不匹配

思路：利用栈的进栈和出栈特性，对于匹配成功的移除

## 创建栈对象

``` js
function Stack() {
    this.arr = []
    // 压栈
    this.push = function (ele) {
        this.arr.push(ele)
    }
    // 出栈
    this.pop = function () {
        return this.arr.pop()
    }
    // 获取栈顶
    this.top = function () {
        return this.arr[this.arr.length - 1]
    }
    // 长度
    this.size = function () {
        return this.arr.length
    }
    // 是否为空
    this.isEmpty = function () {
        return this.arr.length === 0
    }
    this.toString = function () {
        return this.arr.toString()
    }
}
```

## 处理括号匹配

``` js
function matchBracket(str) {
    console.log(`字符串：${str}`)
    let strArr = str.split('')
    // 对数
    let bracketSize = 0
    for (let i = 0; i < strArr.length; i++) {
        if (['[', '{', '('].indexOf(strArr[i]) > -1) {
            bracketStack.push(strArr[i])
        } else if ([']', '}', ')'].indexOf(strArr[i]) > -1) {
            let topEle = bracketStack.top()
            if (topEle === '[' && strArr[i] === ']' || topEle === '{' && strArr[i] === '}' || topEle === '(' && strArr[i] === ')') {
                bracketSize++
                bracketStack.pop()
                console.log('匹配成功')
            } else {
                console.log('括号不匹配')
                break
            }
        }
    }
    if (bracketStack.isEmpty()) {
        console.log(`括号匹配完成，一共有${bracketSize}对`)
    } else {
        console.log('括号不匹配')
    }
    console.log(bracketStack)
}
```

## 传字符串调用

``` js
let bracketStack = new Stack()
matchBracket('[{()}({){()}]') // 不匹配
matchBracket('[{()}(){()}]') // 括号匹配完成，一共有6对
```