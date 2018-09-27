---
title: Markdown基本语法
date: 2018-09-27 14:09:08
tags: ['markdown']
categories: ['markdown']
---
## 1. 标题
__代码__
注：# 后面保持空格
``` markdown
# h1
## h2
### h3
#### h4
##### h5
###### h6
####### h7      // 错误代码
######## h8     // 错误代码
######### h9    // 错误代码
########## h10  // 错误代码
```
效果：
# h1
## h2
### h3
#### h4
##### h5
###### h6
####### h7      // 错误代码
######## h8     // 错误代码
######### h9    // 错误代码
########## h10  // 错误代码
---

## 2. 分级标题

__代码__
注：``=`` ``-`` 最少可以只写一个，兼容性一般

``` markdown
一级标题
======================
二级标题
---------------------
```
效果：
一级标题
======================
二级标题
---------------------

---

## 3. TOC
注：根据标题生成目录，兼容性一般

__代码__

``` markdown 
[TOC]
```
效果：
[TOC]

---

## 4. 引用
__代码1(单行式)__
``` markdown
> hello world!
```
效果
> hello world!

__代码2(多行式)__
``` markdown
> hello world!
hello world!
hello world!    
```

或者

``` markdown
> hello world!
> hello world!
> hello world!
```
相同效果
> hello world!
hello world!
hello world!    

__代码3(多层嵌套)__

``` markdown
> aaaaaaaaa
>> bbbbbbbbb
>>> cccccccccc
```
__演示__

> aaaaaaaaa
>> bbbbbbbbb
>>> cccccccccc
---
## 5. 行内标记
注：用 ` 标记代码块将变成一行
__代码__
``` markdown
标记之外`hello world`标记之外
```
__演示__
标记之外`hello world`标记之外

__错误代码__
注：不同平台错误略有差异

``` markdown
 标记之外 ` 
< div>   
    < div></div>
    < div></div>
    < div></div>
< /div>
`标记之外
```
__演示__
 标记之外 ` 
< div>   
    < div></div>
    < div></div>
    < div></div>
< /div>
`标记之外

---
## 6. 代码块
注：与上行距离一空行

#### 代码1<code>(```)</code>


注：用```生成块
``` markdown
` ``
<div>   
    <div></div>
    <div></div>
    <div></div>
</div>
` ``
```
__效果__
```
<div>   
    <div></div>
    <div></div>
    <div></div>
</div>
```
#### 代码2(Tab)
注： Tab缩进
``` html
我是文字……

    <div>   
        <div></div>
        <div></div>
        <div></div>
    </div>
```
__效果__
``` html
<div>   
    <div></div>
    <div></div>
    <div></div>
</div>
```
#### 代码3(自定义语法)
注：根据不同的语言配置不同的代码着色
``` javascript
`` ` js
var num = 0;
for (var i = 0; i < 5; i++) {
    num+=i;
}
console.log(num);
`` `
```

__效果__

``` javascript
var num = 0;
for (var i = 0; i < 5; i++) {
    num+=i;
}
console.log(num);
```
---
## 7. 插入链接

#### 代码1(内链式)
注：``{:target="_blank"}``跳转方式兼容性一般 ，多数第三方平台不支持跳转
``` markdown
[百度1](http://www.baidu.com/"百度一下")
```
__效果__


[百度1](http://www.baidu.com/'百度一下')
<a href="http://www.baidu.com/'百度一下'" target="_blank">百度2</a> 

---
## 8. 插入图片
#### 代码1(内链式)
``` markdown
![](./01.png '描述')
```
__ 效果__

![](https://upload-images.jianshu.io/upload_images/6912209-8c53b79a706bb7c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/273/format/webp)

---
## 9. 插入图片带有链接
__代码__
``` markdown
[![](./01.png '百度')](http://www.baidu.com){:target="_blank"}        // 内链式

[![](./01.png '百度')][5]{:target="_blank"}                       // 引用式
[5]: http://www.baidu.com
```
__效果__

[![](https://upload-images.jianshu.io/upload_images/6912209-8c53b79a706bb7c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/273/format/webp '百度')](http://www.baidu.com)
---
## 10. 视频插入
注：``Markdown`` 语法是不支持直接插入视频的
普遍的做法是 插入``HTML``的 ``iframe`` 框架，通过网站自带的分享功能获取，如果没有可以尝试第二种方法
第二是伪造播放界面，实质是插入视频图片，然后通过点击跳转到相关页面

#### 代码1
注：多数第三方平台不支持插入``<iframe>``视频
![](https://upload-images.jianshu.io/upload_images/6912209-29337f2896bf4629.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/621/format/webp)

``` markdown
<iframe height=498 width=510 src='http://player.youku.com/embed/XMjgzNzM0NTYxNg==' frameborder=0 'allowfullscreen'></iframe>
```
__效果__
<iframe height=498 width=510 src='http://player.youku.com/embed/XMjgzNzM0NTYxNg==' frameborder=0 'allowfullscreen'></iframe>

#### 代码2
``` markdown
[![](./youku2.png)](http://v.youku.com/v_show/id_XMjgzNzM0NTYxNg==.html?spm=a2htv.20009910.contentHolderUnit2.A&from=y1.3-tv-grid-1007-9910.86804.1-2#paction){:target="_blank"}
```
[![](https://upload-images.jianshu.io/upload_images/6912209-d11325d111b86cc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/508/format/webp)](http://v.youku.com/v_show/id_XMjgzNzM0NTYxNg==.html?spm=a2htv.20009910.contentHolderUnit2.A&from=y1.3-tv-grid-1007-9910.86804.1-2#paction)

---

## 11. 序表
#### 代码1(有序)
注：序列``.``后 保持空格
``` markdown
1. one
2. two
3. three
```
__效果__
1. one
2. two
3. three

#### 代码2(无序)
``` markdown
* one
* two
* three
```
__效果__
* one
* two
* three

#### 代码3(序表嵌套)
``` markdown
1. one
    1. one-1
    2. two-2
2. two 
    * two-1
    * two-2
```
__效果__
1. one
    1. one-1
    2. two-2
2. two 
    * two-1
    * two-2

#### 代码4(序表嵌套代码块)
注：换行+两个Tab
``` markdown
* one

    var a = 10;     // 与上行保持空行并 递进缩进
```
__效果__
* one

        var a = 10;     // 与上行保持空行并 递进缩进
---
## 12. 任务列表
注：兼容性一般 要隔开一行
__代码__
``` markdown
这是文字……

- [x] 选项一
- [ ] 选项二  
- [ ]  [选项3]
```
__效果__
这是文字……

- [x] 选项一
- [x] 选项二  
- [ ]  [选项3]

---
## 13. 表情

注：兼容一般

😁😢🐑🐂🍎👁
---
## 14. 表格
注： ``:`` 代表对齐方式 ,** ``:`` 与 ``|`` 之间不要有空格**，否则对齐会有些不兼容

#### 代码1
``` markdown
|    a    |       b       |      c     |
|:-------:|:------------- | ----------:|
|   居中  |     左对齐    |   右对齐   |
|=========|===============|============|
```
__效果__
|    a    |       b       |      c     |
|:-------:|:------------- | ----------:|
|   居中  |     左对齐    |   右对齐   |
|=========|===============|============|

#### 代码2(简约写法)
``` markdown
a  | b | c  
:-:|:- |-:
    居中    |     左对齐      |   右对齐    
============|=================|=============
```
__效果__

a| b | c
:-:|:- |-:
    居中    |     左对齐      |   右对齐    
============|=================|=============

---
## 15. 支持内嵌CSS样式
__代码__
``` html
<p style="color: #AD5D0F;font-size: 30px; font-family: '宋体';">内联样式</p>
```
__效果__
<p style="color: #AD5D0F;font-size: 30px; font-family: '宋体';">内联样式</p>

---

## 16. 语义标记

|    __描述__    |       __效果__       |      __代码__     |
|:-------:|:------------- | :----------|
|   居中  |     左对齐    |   右对齐   |
|斜体|	斜体|	*斜体*
|斜体|	斜体|	_斜体_
|加粗|	加粗|	**加粗**
|加粗+斜体|	加粗+斜体|	***加粗+斜体***
|加粗+斜体|	加粗+斜体|	**_加粗+斜体_**
|删除线|	删除线|	~~删除线~~

## 17. 语义标签

|    __描述__    |       __效果__       |      __代码__     |
|:-------:|:------------- | :----------|
|斜体|	<i>斜体</i>	|``<i>斜体</i>``
|加粗|	<b>加粗</b>|	``<b>加粗</b>``
|强调|	<em>强调</em>|``	<em>强调</em>``
|上标|	Za|	``Z<sup>a</sup>``
|下标|	Za|	``Z<sub>a</sub>``
|键盘文本|	<kbd>Ctrl</kbd>| ``<kbd>Ctrl</kbd>``
|换行	|	

## 18. 格式化文本
__保持输入排版格式不变__
注：对内含标签需要破坏结构才能显示
__代码__
``` markdown
<pre>
hello world 
        hi
hello world 
</pre>
```
__效果__
<pre>
hello world 
        hi
hello world 
</pre>
__错误解决方法__
注：标签内部添加空格 或者 直接使用```标记来处理
#### 代码1(插入空格)

``` markdown
<pre>
    < div>   
        < div>< /div>
        < div>< /div>
        < div>< /div>
    < /div>
</pre>
```
__效果__
<pre>
    < div>   
        < div>< /div>
        < div>< /div>
        < div>< /div>
    < /div>
</pre>
#### 代码2( <code>```</code> 代码块标记)

``` markdown
`` `
<div>   
    <div></div>
    <div></div>
    <div></div>
</div>
`` `
```
__效果__
```
<div>   
    <div></div>
    <div></div>
    <div></div>
</div>
```
---
## 19. 公式
注：1个$左对齐，2个居中

__代码__
``` markdown
$$ x \href{why-equal.html}{=} y^2 + 1 $$
$ x = {-b \pm \sqrt{b^2-4ac} \over 2a}. $
```
__效果__
$$ x \href{why-equal.html}{=} y^2 + 1 $$
$ x = {-b \pm \sqrt{b^2-4ac} \over 2a}. $

---
## 20. 分隔符
注：最少三个 ``---`` 或 ``***``或 ``* * *``

__代码__
``` markdown
***
---
* * *
```
__效果__
***
---
* * *

## 21. 脚注
__代码__
``` markdown
Markdown[^1]
[^1]: Markdown是一种纯文本标记语言        // 在文章最后面显示脚注
```
__效果__

Markdown[^100]
[^100]: Markdown是一种纯文本标记语言

---
## 22. 锚点
__代码__
注：只有标题支持锚点， 跳转目录方括号后 保持空格
``` markdown
[公式标题锚点](#1)

### [需要跳转的目录] {#1}    // 方括号后保持空格
// 例如

[公式](#101)
```
<!-- __效果__ -->


---
## 23. 定义型列表
注：解释型定义
__代码__
``` markdown
Markdown 
:   轻量级文本标记语言，可以转换成html，pdf等格式  //  开头一个`:` + `Tab` 或 四个空格

代码块定义
:   代码块定义……

        var a = 10;         // 保持空一行与 递进缩进
```
__效果__
Markdown 
:   轻量级文本标记语言，可以转换成html，pdf等格式  //  开头一个`:` + `Tab` 或 四个空格

代码块定义
:   代码块定义……

        var a = 10;         // 保持空一行与 递进缩进
---
## 24. 自动邮箱链接
__代码__
``` markdown
<540042281@qq.com>
```
__效果__

<540042281@qq.com>
---
## 25. 时序图
__代码__
``` markdown
`` `sequence
A->>B: 你好
Note left of A: 我在左边     // 注释方向，只有左右，没有上下
Note right of B: 我在右边
B-->A: 很高兴认识你
`` `
```
__效果__
```sequence
A->>B: 你好
Note left of A: 我在左边     // 注释方向，只有左右，没有上下
Note right of B: 我在右边
B-->A: 很高兴认识你
```