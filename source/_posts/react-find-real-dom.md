---
title: React查找真实DOM
date: 2018-09-06 11:02:47
tags: ['react']
categories: ['react']
---
组件并不是真实的DOM节点，而是存在于内存之中的一种数据结构，叫做**虚拟DOM（virtual DOM）**。只有当它插入文档以后，才会变成真实的DOM。根据React的设计，所有的DOM变动，都先在虚拟DOM上发生，然后再将实际发生变动的部分，反映在真实DOM上，这种算法叫做**DOM diff**，它可以极大提高网页的性能表现。 
但是，有时需要从组件获取真实DOM的节点，这时就要用到`React.findDOMNode`方法。

``` javascript
 var MyComponent = React.createClass({
    handleClick:function(){
        React.findDOMNode(this.refs.myTextInput).focus();
    },
    render:function(){
        return (
            <div>
                <input type="text" ref="myTextInput" />
                <input type="button" value="Focus the text input" onClick={this.handleClick}/>
            </div>
        );
    }
});

React.render(
    <MyComponent />,
    document.getElementById('example')
);
```

上面代码中，组件MyComponent的子节点有一个文本输入框，用于获取用户的输入。这时就必须获取真实的DOM节点，虚拟DOM是拿不到用户输入的。为了做到这一点，文本输入框必须有一个ref属性，然后`this.refs.[refName]`就指向这个虚拟DOM的子节点，最后通过`React.findDOMNode`方法获取真实DOM的节点。需要注意的是，由于`React.findDOMNode`方法获取的是真实DOM，所以必须等到虚拟DOM插入文档以后才能调用这个方法，否则会返回null。上面代码中，通过为组件指定Click事件的回调函数，确保了只有等到真实DOM发生Click事件之后，才会调用`React.findDOMNode`方法。