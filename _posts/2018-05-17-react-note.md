---
layout: post
title: react学习笔记
categories: react
description: 《深入浅出react和redux》的笔记
keywords: react,学习笔记
---
# 开始记录每章的学习

## Class1 React 和 jquery的不同

jQ即可的方式直观易懂，对于初学者十分适用，但是当项目逐渐变得庞
大时，用 jQuery 写出的代码往往互相纠缠，
[![1-4.png](https://i.loli.net/2018/05/17/5afd573fe55ef.png)](https://i.loli.net/2018/05/17/5afd573fe55ef.png)

使用 React 的方式，就可以避免构建这样复杂的程序结构，无论何种事件，引发的
都是 React 组件的重新渲染，至于如何只修改必要的 DOM 部分，则完全交给 React 去操
作，开发者并不需要关心.

[![1-5.png](https://i.loli.net/2018/05/17/5afd577ba4fe2.png)](https://i.loli.net/2018/05/17/5afd577ba4fe2.png)

## Class2 React的生命周期

### 1.父组件向子组件通过prop来传递数据

例：子组件可通过this.props.a获得a的值。

```React
<Example a={10}/>
```

### 2.组件中的状态用state记录。

通常在构造函数时，使用this.state进行赋值，在需要改变状态时，通过this.setState来进行状态改变，而后渲染界面。例：

```React
constructor(props) {
    super(props);
    this.onClickButton = this.onClickButton.bind(this);
    this.state = { count: 0 };
}

onClickButton() {
    this.setState({count:++this.state.count})
}
```

### 3.生命周期

React 严格定义了组件的生命周期，生命周期可能会经历如下三个过程：

装载过程（Mount ），也就是把组件第一次在 DOM 树中渲染的过程；

更新过程（Update ），当组件被重新渲染的过程；

卸载过程（Unmount ），组件从 DOM 中删除的过程;

每个过程都有一些相对应的函数。

装载过程调用以下几个函数
constructor: 类构造函数getlnitialState: ReactClass的状态初始化(ES6不触发)  
getDefaultProps: ReactClass获得默认props(ES6不触发)  
componentWillMount: 装载渲染前调用  
render: 描述即将渲染JSX的函数  
componentDidMount: 渲染成功后调用（仅浏览器端调用）  

更新过程调用以下几个函数：
componentWillReceiveProps: 调用render后被调用
shouldComponentUpdate: 是否需要渲染组件
componentWillUpdate: 重新渲染前调用
render: 描述即将渲染JSX的函数
componentDidUpdate: 重新渲染成后调用