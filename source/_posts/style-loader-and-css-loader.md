---
title: Webpack学习笔记之style-loader&css-loader
date: 2018-09-26 15:36:03
tags: ['webpack']
categories: ['webpack', 'loader']
---

## 前言
> webpack不仅有打包功能，还能对项目中各种文件按照我们的需求进行处理，这就用到了loader，所谓loader就是集成到webpack的文件处理方法，这些loader在webpack打包过程中，可以对指定类型的文件进行相应的处理，比如把less语法转换成浏览器可以识别的css语法，引入特定类型的文件（html）等等。下面将介绍一下一系列常用的loader。本文介绍style-loader和css-loader。

webpack可以把以指定入口的一系列相互依赖的模块打包成一个文件，这里的模块指的不只是js，也可以是css。也就是说，当你用CommonJs规范去引用css文件时，webpack会把你引用的css文件也打包到最终的生成文件中里。这样我们如何让样式生效呢？有两种方法：一种是，在引入css时，在最后生成的js文件中进行处理，动态创建style标签，塞到head标签里。这样，html页面引用这个js文件时，就可以让样式生效了。另一种方法是，打包时把css文件拆出来，css相关模块最终打包到一个指定的css文件中，我们手动用link标签去引入这个css文件就可以了。这两种方法都需要配置响应的loader。这篇文章介绍第一种方法

举个栗子🌰
## __1. 在src文件中，创建main.css__

main.css代码如下：

``` css
#container {
    width: 200px;
    height: 100px;
    margin: auto;
    border: 1px solid;
    display: flex;
}
#container .button {
    width: 100px;
    height: 30px;
    line-height: 30px;
    text-align: center;
    margin: auto;
    border: 1px solid;
    cursor: pointer;
}
```

---

## __2. 然后在入口文件./src/main.js中引用之：__
``` js
require('./main.css');
var greet = require('./greet.js');
/**
 * 为按钮绑定弹框问候
 */
function bindButtonElementEvent(btnElement) {
    btnElement.addEventListener('click', function () {
        greet();
    });
}
 
window.bindButtonElementEvent = bindButtonElementEvent;
```
---

## __3. 使用npm安装style-loader和css-loader，这两个loader的作用会在下面介绍__

``` shell
npm install --save-dev style-loader css-loader
```

---
## __4. 在webpack.config.js里配置loader__
这里简要介绍一下loader配置语法，看下面的例子：

``` js
// webpack.config.js
module.exports = {
 
    entry: __dirname + '/src/main.js',
 
    output: {
        path: __dirname + '/output',
        filename: 'main.js'
    },
 
    module: {
        loaders: [
            {
                test: /\.css$/,
                loader: 'style-loaer!css-loader'
            }
        ]
    }
}

```
上面的配置中，loaders是一个数组，其中的元素是我们使用的所有loader，每个loader对应一个object，object的属性中，test是加载器要匹配的文件后缀正则，loader是使用的加载器，“!”用来分隔不同的加载器。上述loader配置表示，webpack在打包过程中，遇到后缀为css的文件，就会使用style-loader和css-loader去加载这个文件。

__上面的loader配置是webpack1的写法，对应的webpack2写法如下（由于有些webpack1写法已经不被webpack2支持，因此建议使用webpack2的写法写配置文件，本文后续将使用webpack2的写法）：__

```js
module.exports = {
 
    entry: __dirname + '/src/main.js',
 
    output: {
        path: __dirname + '/output',
        filename: 'main.js'
    },
 
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            }
        ]
    }
}
```
那么，``css-loader``和``style-loader``是干什么用的呢？

引用《入门webpack》中的原文：__``css-loader``使你能够使用类似@import和url（...）的方法实现require的功能，``style-loader``将所有的计算后的样式加入页面中，二者组合在一起使你能够把样式表嵌入webpack打包后的js文件中。__

因此，我们这样配置后，遇到后缀为.css的文件，``webpack``先用``css-loader``加载器去解析这个文件，遇到“``@import``”“``url``”等语句就将相应样式文件引入（所以如果没有``css-loader``，就没法解析这类语句），最后计算完的css，将会使用``style-loader``生成一个内容为最终解析完的css代码的style标签，放到head标签里。
__需要注意的是，loader是有顺序的，webpack肯定是先将所有css模块依赖解析完得到计算结果再创建style标签。因此应该把``style-loader``放在``css-loader``的前面（webpack loader的执行顺序是从右到左）。__

---

## __5. 打包__

命令行输入：
``` shell
npm start
```
打包后，刷新页面，可以看到，已经有了样式了。

---

## __6. css-loader options__
* __alias__: 解析别名
* __importLoader(@import)__
* __Minimize__: ``true`` or ``false``,是否开启css代码压缩，比如压缩空格不换行。
* __modules__：是否开启``css-modules``

---

## __7. style-loader && style-loader options__
``style-loader``分类


* __style-loader__:配合``css-loader``使用，以``<style></style>``形式在html页面中插入css代码
  
* __style-loader/url__： 以``link``标签形式向``html``页面中插入代码，采用这种方式需要将``css-loader``变为``file-loader``,但这种方式不推荐，因为如果在一个js文件中引入多个``css``文件会生成多个``link``标签，而``html``每个``link``标签都会发送一次网络请求，所以这种方式并不建议。
  
* __style-loader/useable__: 采取这种方式使用处理``css``，会有``use()``和``unuse()``l两种方法，``use()``开启引入样式，``unuse()``不适用样式。


__options__ 选项

 * __attrs__: 添加自定义 ``attrs`` 到 ``style`` 标签 
 * __insertAt__:插入位置 
 * __insertInto__: 插入到指定``dom`` 
 * __singleton__:类型为布尔值，多个样式是否只生成一个``<style></style>``标签。
 * __transform__:根据给定逻辑在css插入html前选择指定样式

---

## __8. ``options``__ 栗子🌰：
``attrs``: ``attrs``是一个对象，以键值对出现，在``<style></style>``标签中以``key = value``出现，键值对可以自定义，但是使用时建议语义化。

``` js
//style-loader添加options选项
{
    loader: 'style-laoder',
    options: {
        attrs: {  //attrs是一个对象，以键值对出现，建议语义化
            first: '1'
        }
    }
}
```
可以看到，此时只有一个标签插入到文档中。common.css和base.css模块的样式被合并到一个样式标签中。


![1.jpeg](https://raw.githubusercontent.com/wangmingdong/docImg/master/style-loader-and-css-loader/1.jpeg)

可以看到``base.css``和``common.css``对应的两个``<style></style> ``标签都被添加了``first = "1"``

接下来我们给``style-loader``的``options``增加``singleton``属性,IE9对页面上``style``标签数量有严格限制，所以这个属性还是很有用的。

``` js
// webpack.config.js
use:[
        {
        loader: 'style-loader',
        options: {
            attrs: {
            first: 1
            },
            singleton: true
        }
        },
        {
        loader: 'css-loader'

        }
    ]
```

可以看到，此时只有一个标签插入到文档中。common.css和base.css模块的样式被合并到一个样式标签中。

![2.png](https://raw.githubusercontent.com/wangmingdong/docImg/master/style-loader-and-css-loader/2.png)

上面提到``options``选项还有``insertAt``属性，``insertAt``有两个值``top ``| ``bottom``,如果不配置``insertAt``则 默认为``bottom``，当``insertAt``： ‘``top``’ 时 ，``loader``打包的``css``将优先已经存在的``css``，``insertInto``插入到指定标签。

在html页面中添加新的样式

``` html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>demo 01</title>
  <style>
    html {  //背景色设置为红色
      background: red
    }
  </style>
</head>
```

然后insertAt: ‘top’打包后可以发现webpack打包加载生成的标签会在标签的上方。背景色会被覆盖为红色。 

![3.png](https://raw.githubusercontent.com/wangmingdong/docImg/master/style-loader-and-css-loader/3.png)

在html中新建``` <div id="app"></div> ```标签，利用insertInto将打包的css插入到该div中。

![4.png](https://raw.githubusercontent.com/wangmingdong/docImg/master/style-loader-and-css-loader/4.png)