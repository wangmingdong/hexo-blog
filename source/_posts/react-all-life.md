---
title: React生命周期
date: 2018-09-06 09:48:06
tags: ['react']
categories: ['react']
---
### 组件的生命周期
组件的生命周期分成三个状态：
* Mounting:已插入真实DOM
* Updating：正在被重新渲染
* Unmounting：已移出真实DOM
  
React为每个状态都提出了两种处理函数，will函数在进入状态之前调用，did函数在进入状态之后调用，三种状态共计五种处理函数：

* componentWillMount()
* componentDidMount()渲染到dom树中时调用，可用于获取原生节点
* componentWillUpdate(object nextProps, object nextState)
* componentDidUpdate(object prevProps, object prevState)
* componentWillUnmount()组件从dom销毁前调用

此外，React还提供两种特殊状态的处理函数
* componentWillReceiveProps(object nextProps):已加载组件收到新的参数时调用
* shouldComponentUpdate(object nextProps,object nextState):组件判断是否重新渲染时调用

``` javascript
var Hello = React.createClass({
    getInitialState:function(){
        return {opacity:1.0};
    },
    componentDidMount:function(){
        this.timer = setInterval(function(){
            var opacity = this.state.opacity;
            opacity -= .05;
            if(opacity < 0.1){
                opacity = 1.0;
            }
            this.setState({
                opacity:opacity
            });
        }.bind(this),100);
    },
    render:function(){
        return (
            <div style={{opacity:this.state.opacity}}>
                Hello {this.props.name}
            </div>
        );
    }
});

React.render(
    <Hello name="world" />,
    document.getElementById('example')
);
```
上面代码在hello组件加载以后，通过componentDidMount方法设置一个定时器每隔100毫秒，就重新设置组件的透明度，从而引发重新渲染。 
另外，组件的style属性的设置方式也值得注意，不能写成

``` javascript
style="opacity:{this.state.opacity};"
```

而要写成

``` javascript
style={{opacity:this.state.opacity}}
```

这是因为 React组件样式 是一个对象，所以第一重大括号表示这是JavaScript，第二重大括号表示样式对象。