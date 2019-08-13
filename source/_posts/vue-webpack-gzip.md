---
title: vue中webpack如何开启gzip
date: 2019-08-13 17:45:58
tags: ["vue", 'webpack']
categories: ['vue']
---

如果使用vue-cli构建项目，会自动生成webpack相关配置。
如下图找到对应文件，修改 productionGzip 为 true，开启Gzip压缩

![1](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-08-13-vue-webpack-gzip/1.png)

找到下图webpack配置，修改如下：

![2](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-08-13-vue-webpack-gzip/2.png)

``` js
if (config.build.productionGzip) {
  const CompressionWebpackPlugin = require('compression-webpack-plugin')

  webpackConfig.plugins.push(
    new CompressionWebpackPlugin({
      asset: '[path].gz[query]',
      algorithm: 'gzip',
      test: new RegExp(
        '\\.(' +
        config.build.productionGzipExtensions.join('|') +
        ')$'
      ),
      threshold: 10240,
      minRatio: 0.8
    })
  )
}
```

当然，需要先安装依赖：
``` bash
npm install --save-dev compression-webpack-plugin
// 在没有给定版本号时该命令默认安装最新版compression-webpack-plugin，问题恰恰就出在这。
```

安装后执行 npm run build，然后就报错了：

``` bash
...
throw new ValidationError(ajv.errors, name);
ValidationError: Compression Plugin Invalid Options
options should NOT hava additional properties
...
```
最后去官网 [ https://www.npmjs.com/package/compression-webpack-plugin ]( https://www.npmjs.com/package/compression-webpack-plugin )得知，更新的v2.0.0之后选项属性和v1.x不匹配。。。

> Requirements：This module requires a minimum of Node v6.9.0 and Webpack v4.0.0

我的webpack版本 v3.x，明显不支持么！
没得法子卸载重装compression-webpack-plugin@1.x吧

``` bash
npm uninstall --save-dev compression-webpack-plugin
```
安装v1.1.12版本（该版本为1.1最新版）
```
npm install --save-dev compression-webpack-plugin@1.1.12　　　　//记得带版本号
```


至此 ，webpack设置算是结束了，下面是服务器设置
___

以nginx为例：

找到conf目录下的nginx.conf ,开启gzip,并设置gzip的类型，如下

``` bash
gzip  on;
gzip_types text/plain application/x-javascript application/javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
```
重新部署服务器
> nginx -s reload