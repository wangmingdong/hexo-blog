---
title: 如何判断Angular中ng-repeat的dom渲染完成
date: 2018-05-30 17:54:34
tags: ["angular", 'ng-repeat', 'directive']
categories: ['angular']
---

> 最近在做项目中碰到一个问题，当ng-repeat较多循环较慢出现明显的卡顿时，加载loading效果何时开始何时结束。

在绑定ng-repeat的标签上写上自定义标签，如：

``` html
<ul>
    <li ng-repeat=“item in items” repeat-finish=“renderFinish();”>
    {{item.name}}
    </li>
</ul>
```
directive中：
``` js
app.directive('repeatFinish',function(){
    return {
        link: function(scope,element,attr){
            if(scope.$last == true){
                console.log('ng-repeat执行完毕')
                scope.$eval( attr.repeatFinish )
            }
        }
    }
})
```
controller中：
``` js
//controller里对应的处理函数
$scope.renderFinish = function(){
    console.log('渲染完之后的操作')
}
```