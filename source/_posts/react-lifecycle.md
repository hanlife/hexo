---
title: 探索React生命周期
comments: true
date: 2018-07-19 15:57:42
categories: React
tags: [
    React,
]
---

# React生命周期

在组件的整个生命周期中，随着该组件的props或者state发生改变，其DOM表现也会有相应的变化。一个组件就是一个状态机，对于特定地输入，它总返回一致的输出。

## 组件的生命周期如下
- 实例化
- 存在期
- 销毁期

![image](/image/ajs-life.png)

<!-- more -->

### 实例化


```
var List = React.createClass({
    //1.创建阶段
    getDefaultProps:function() {
        console.log("getDefaultProps");
        return {};
    },
 
    //2.实例化阶段
    getInitialState:function() {
        console.log("getInitialState");
        return {};
    },
 
    //render之前调用，业务逻辑都应该放在这里，如对state的操作等
    componentWillMount:function() {
        console.log("componentWillMount");
    },
 
    //渲染并返回一个虚拟DOM
    render:function() {
        console.log("render");
        return(
            <div> hello <strong> {this.props.name} </strong></div>
            );
    },
 
    //该方法发生在render方法之后。在该方法中，ReactJS会使用render生成返回的虚拟DOM对象来创建真实的DOM结构
    componentDidMount:function() {
        console.log("componentDidMount");
    },
});

```
其输出结果就是：

```
getDefaultProps
getInitialState
componentWillMount
render
componentDidMount
```
- getInitialState 初始化组件的state的值，其返回值会赋值给组件的this.state属性。对于组件的每个实例来说，这个方法的调用次数有且只有一次。与getDefaultProps方法不同的是，每次实例创建时该方法都会被调用一次。
- componentWillMount 此方法会在完成首次渲染之前被调用。这也是在render方法调用前可以修改组件state的最后一次机会。
- render 生成页面需要的虚拟DOM结构，用来表示组件的输出。render方法需要满足：（1）只能通过this.props和this.state访问数据；（2）可以返回null、false或者任何React组件；（3）只能出现一个顶级组件；（4）必需纯净，意味着不能改变组件的状态或者修改DOM的输出。
- componentDidMount 该方法发生在render方法成功调用并且真实的DOM已经被渲染之后，在该函数内部可以通过this.getDOMNode()来获取当前组件的节点。然后就可以像Web开发中的那样操作里面的DOM元素了。

---

#### 创建阶段

这个阶段只会触发一个getDefaultProps方法，该方法返回一个对象，并且缓存下来。然后与父组件指定的props对象合并，最后赋值给this.props作为该组件的默认属性。对于那些没有被父辈组件指定的props属性的新建实例来说，这个方法返回的对象可用于为实例设置默认的props值。</br>
props属性又是什么呢，它是一个对象，是组件用来接收外面传来的参数的，组件内部是不允许修改自己的props属性的，只能通过父组件来修改。在getDefaultProps方法中，是可以设定props默认值的。

---

#### 实例化阶段

该阶段主要发生在实例化组件类的时候，也就是该组件类被调用的时候：

```
ReactDOM.render(<NewView name="ReactJS">children</NewView>, document.body);
```

---

#### 存在期


```

var NewView = React.createClass({
    //1.创建阶段
    getDefaultProps:function() {
        console.log("getDefaultProps");
        return {};
    },
 
    //2.实例化阶段
    getInitialState:function() {
        console.log("getInitialState");
        return {
            num:1
        };
    },
 
    //render之前调用，业务逻辑都应该放在这里，如对state的操作等
    componentWillMount:function() {
        console.log("componentWillMount");
    },
 
    //渲染并返回一个虚拟DOM
    render:function() {
        console.log("render");
        return(
            <div> hello <strong> {this.props.name} </strong>
                <button onClick={this.handleAddNumber}> hello <strong> {this.state.num} </strong></button>
            </div>
            );
    },
 
    //该方法发生在render方法之后。在该方法中，ReactJS会使用render生成返回的虚拟DOM对象来创建真实的DOM结构
    componentDidMount:function() {
        console.log("componentDidMount");
    },
 
    //3.更新阶段
    componentWillReceiveProps:function() {
        console.log("componentWillReceiveProps");
    },
 
    //是否需要更新
    shouldComponentUpdate:function() {
        console.log("shouldComponentUpdate");
        return true;
    },
 
    //将要更新
    componentWillUpdate:function() {
        console.log("componentWillUpdate");
    },
 
    //更新完毕
    componentDidUpdate:function() {
        console.log("componentDidUpdate");
    },
 
    //4.销毁阶段
    componentWillUnmount:function() {
        console.log("componentWillUnmount");
    },
 
    // 处理点击事件
    handleAddNumber:function() {
        console.log("add num");
        this.setState({num:this.state.num+1});
        this.setProps({name:"newName"});
    }
});

```

输出结果为(不包含)：


```
add num
componentWillReceiveProps
shouldComponentUpdate
componentWillUpdate
render
componentDidUpdate
```

- componentWillReceiveProps 在任意时刻，组件的props都可以通过父辈组件来更改。当组件接收到新的props(这里不同于state）时，会触发该函数，我们同时也获得更改props对象及更新state的机会。
- shouldComponentUpdate 该方法用来拦截新的props和state，然后开发者可以根据自己设定逻辑，做出要不要更新render的决定，让它更快。
- componentWillUpdate 与componentWillMount方法类似，组件上会接收到新的props或者state渲染之前，调用该方法。但是不可以在该方法中更新state和props。
- render 生成页面需要的虚拟DOM结构，并返回该结构
componentDidUpdate 与componentDidMount类似，更新已经渲染好的DOM。

---

#### 生命周期之销毁&清理

每当react使用完一个组件，这个组件就必须从DOM中卸载随后被销毁。此时，仅有的一个钩子函数会做出响应，完成所有的清理与销毁工作，这很必要。</br>

###### componentWillUnmount

最后，随着一个组件从它的层级结构中移除，这个组件的生命也就走到了尽头。该方法会在组件被移出之前调被调用。在componentDidMount方法中添加的所有任务都需要在该方法中撤销，比如说创建的定时器或者添加的事件监听等。
