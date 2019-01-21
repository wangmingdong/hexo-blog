---
title: 使用Angular中的$sce
date: 2019-01-21 16:44:55
tags: ['angular']
categories: ['angular']
---

为什么要要``sce``？因为angularJS里好些地方，比如路径默认是个字符串，当做变量传进src中不会认为是路径而是字符串，初始化dom时会在console中报错找不到资源；
那么我们就可以通过$sce告诉angualrJS这个路径，这样是很安全。它有以下几种：

``` js
$sce.trustAs(type,name); 
$sce.trustAsUrl(value);
$sce.trustAsHtml(value);    // 解释成html标签
$sce.trustAsResourceUrl(value); // 解释成资源路径
$sce.trustAsJs(value);
```
后面几个api都是基于第一个trustAs调用的，比如trsutAsUrl其实调用的是trsutAs($sce.URL,"xxxx");

其中 type 可选的值为：

``` js
$sce.HTML
$sce.CSS
$sce.URL // a标签中的href ， img标签中的src
$sce.RESOURCE_URL // ng-include,src或者ngSrc，比如iframe或者Object
$sce.JS
```
### 来自官网的例子：ng-bind-html
``` html
<!DOCTYPE html>
<html>
<head>
  <title></title>
  <script src="http://apps.bdimg.com/libs/angular.js/1.2.16/angular.min.js"></script>
</head>
<body ng-app="mySceApp">
  <div ng-controller="AppController">
   <i ng-bind-html="explicitlyTrustedHtml" id="explicitlyTrustedHtml"></i>
  </div>
  <script type="text/javascript">
    angular.module('mySceApp',[])
    .controller('AppController', ['$scope', '$sce',
     function($scope, $sce) {
      $scope.explicitlyTrustedHtml = $sce.trustAsHtml(
        '<span onmouseover="this.textContent="Explicitly trusted HTML bypasses ' +
        'sanitization."">Hover over this text.</span>');
     }]);
  </script>
</body>
</html>
```
### 工作中遇到的例子：trustAsResourceUrl
filter:
``` js
AppFilter.filter("trustAsRes", function ($sce) {
    return function (res) {
        return $sce.trustAsResourceUrl(res)
    }
});
```

html:
``` html
<audio preload="auto" controls="">
    <source ng-src="{{audio.src | trustAsRes}}" controls="controls">
</audio>
<img ng-src='{{post.fileUrl | trustAsRes}}'/>
```


