---
title: JS数组sort()如何根据两个条件进行排序
date: 2018-06-11 15:08:39
tags: ["js", 'Array', 'sort']
categories: ['js']
---
例如有数组：
``` js
var items = [
  { name: 'Edward',sex: 0, value: 21 },
  { name: 'Sharpe',sex: 1,  value: 37 },
  { name: 'And',sex: 0,  value: 45 },
  { name: 'The',sex: 1,  value: -12 },
  { name: 'Magnetic',sex: 0,  value: 3 },
  { name: 'Zeros',sex: 0,  value: 38 }
];
```
如果需要根据性别并且按照value升序排列:
``` js
items.sort(function (a, b) {
    if (a.sex == b.sex) {
        return (a.value - b.value);
    } else {
        return a.sex - b.sex;
    }
});
console.log(items)
```
结果：
``` js
[
    {name: "Magnetic", sex: 0, value: 3},
    {name: "Edward", sex: 0, value: 21},
    {name: "Zeros", sex: 0, value: 38},
    {name: "And", sex: 0, value: 45},
    {name: "The", sex: 1, value: -12},
    {name: "Sharpe", sex: 1, value: 37}
]
```