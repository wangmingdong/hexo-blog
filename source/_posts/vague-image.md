---
title: 'CSS设置背景图片模糊 文字不模糊'
date: 2018-02-26 22:00:49
tags: ["css3"]
categories: ['css']
---

### 需求

一个div设置了background: url，现在需要使图片背景模糊，div内的文字清晰显示

### 思路

``` css
filter:(2px)
```
在父容器中设置背景，并且使用相对定位，方便伪元素重叠。而在:after中只需要继承背景，并且设置模糊，绝对定位覆盖父元素即可。这样父容器中的子元素便可不受模糊度影响。因为伪元素的模糊度不能被父元素的子代继承。 
说了这么多，来点代码提提神。

### 方法

html:

``` html
<div class="bg">
   <div class="drag">like window</div>
</div>
```
css:
``` css
/*背景模糊*/
.bg{
    width:100%;
    height:100%;
    position: relative;
    background: url("../image/banner/banner.jpg") no-repeat fixed;
    padding:1px;
    box-sizing:border-box;
    z-index:1;
}
.bg:after{
    content: "";
    width:100%;
    height:100%;
    position: absolute;
    left:0;
    top:0;
    background: inherit;
    filter: blur(4px);
    z-index: 2;
}
.drag{
    position: absolute;
    left:50%;
    top:50%;
    transform: translate(-50%,-50%);
    width:200px;
    height:200px;
    text-align: center;

    z-index:11;
}
```
### 效果
![背景图模糊](https://raw.githubusercontent.com/wangmingdong/docImg/master/%E8%83%8C%E6%99%AF%E5%9B%BE%E6%A8%A1%E7%B3%8A1.png)

