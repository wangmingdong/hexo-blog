---
title: 使用cross-env分析项目
date: 2019-08-13 18:20:10
tags: ["vue", 'cross-env']
categories: ['工具']
---

今天为了优化代码，找到一个很有用的工具，能够可视化的帮助分析项目的体积，因为在开发中我们可能会无意思试用或者尝试一些第三方组件，但是这个组件并非项目必须或者不可替代，所以在项目结尾（优化）阶段就特别希望有这么一个工具来帮助分析了~~~

这个神器是一个npm包cross-env，通过npm执行：

``` bash
npm install cross-env --save-dev
```

然后在package.json配置文件scripts节中添加

```
"analyze":"cross-env NODE_ENV=production npm_config_report=true npm run build"
```

最后在终端输入命令：

``` bash
npm run analyze
```

会自动打开浏览器（默认地址：http://127.0.0.1:8888/）展示分析结果，如下图：


![1](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-08-13-cross-env-tool/1.png)

![2](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-08-13-cross-env-tool/2.png)

可以一目了然的看到各个模块的包含关系，最上面的就是项目生成（打包压缩好）的项目文件

