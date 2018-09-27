---
title: 说一说盒子模型的两种类型
date: 2018-09-26 14:43:18
tags: ['css', 'box-sizing']
categories: ['css']
---
## 写在前面
盒子模型应该是老生常谈的问题，最近翻找这类博客时发现还是容易搞混，特别是计算宽高。

box-sizing是css3增加的属性，分为两种类型：__w3c的标准盒模型__和__IE盒模型__，涉及的属性有： ``content`` ,``border`` ,``margin`` , ``padding`` 。

## W3C的标准盒模型（content-box）

![W3C的标准盒模型对应图](https://raw.githubusercontent.com/wangmingdong/docImg/master/boxSizingContent.bmp)

元素的width = content的宽度，
真实占位宽度为 content + padding + border，不要懵逼，这有个栗子🌰：

``` css
.test1{
	box-sizing:content-box;
	width:200px;
	padding:10px;
	border:15px solid #eee;
}
```

![W3C的标准盒模型对应图](https://raw.githubusercontent.com/wangmingdong/docImg/master/boxSizingContent2.png)

真实宽度 width = content(200px) + padding(20px) + border(30px) = 250px
## IE盒子模型（border-box）

![IE盒子模型对应图](https://raw.githubusercontent.com/wangmingdong/docImg/master/boxSizingBorder.png)


元素的width = content + padding + border,
然而它真实的内容宽度 = width - padding - border
举个🌰：

``` css
.test1{
	box-sizing:border-box;
	width:200px;
	padding:10px;
	border:15px solid #eee;
}
```

![W3C的标准盒模型对应图](https://raw.githubusercontent.com/wangmingdong/docImg/master/boxSizingBorder2)

内容content宽度 = width(200px) - padding(20px) - border(30px) = 150px

## 第三种类型（inherit）

继承 父元素 box-sizing属性的值