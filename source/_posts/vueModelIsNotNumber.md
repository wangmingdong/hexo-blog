---
layout: vue
title: Vue的model数据格式无法绑定为Number
date: 2018-02-01 16:30:29
tags: ['vue', 'model']
categories: ['vue']
---
项目中碰到一个小问题，使用vue model时需要的数据格式为number，采用常规方式获取到的永远都是string，后来发现需要在v-model后面加上  .number  如：
``` html
<el-input v-model.number="id"></el-input>
```