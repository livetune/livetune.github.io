---  
layout: post  
title: React Context 的使用
categories: notes,react  
description:  React Context 的使用
keywords: notes,react  
---  

Context 是 React 中一个比较常见的 Api，通常用于跨组件的通讯。Redux 是一个 js 状态容器，配合着Context Api 提供了可预测的状态管理。

## 简介
在 React 应用中，数据的传递通常都是单向数据流（通过 props 实现父子组件通讯），当一个层级比较深的组件需要用到比较靠外的父组件的数据是，props 可能需要经过多个组件逐层传递才能到达那个组件里，而 Context 则提供了在这些组件之间共享数据的方式

### 何时使用Context
Context 用于在各种组件事件共享一些全局的数据，如登录状态、主题、国际化等。如下面是一个显示登录状态的例子，我们可以修改 **loginState** 这个变量来控制 Login 组件是否登录

```js 
import React, { useContext } from 'react'
const Context = React.createContext('loginState')
function Login() {
  const isLogin = useContext(Context)
  return isLogin ? 'login' : 'need to login'
}
function DashBorad() {
  return <Login></Login>
}

function Home() {
  const loginState = false
  return (
    <Context.Provider value={loginState}>
      <DashBorad></DashBorad>
    </Context.Provider>
  )
}
export default Home
```


### 使用 Context 之前的考虑
Context 的主要场景是在不同层级的组件之间访问相同的一些数据，但是会使组件的复用性变差，

如果只是想要避免层层传递 props，组件组合有的时候会是一个更好的方案。下面的例子就是直接将DashBoard 这个组件作为 props 传递下去了，当props多的时候可以有效的减少 props的数量，复杂的场景也可以使用render props。

```js
import React from 'react'

function Login({ isLogin }) {
  return isLogin ? 'login' : 'need to login'
}
function DashBoard() {
  return <Login></Login>
}

function HomeLayout({ dashBoard }) {
  return dashBoard
}

function Home() {
  const loginState = false
  const dashBoard = (
    <DashBoard isLogin={loginState}></DashBoard>
  )
  return (
    <HomeLayout dashBoard={dashBoard}></HomeLayout>
  )
}

export default Home
```

### Api

#### React.createContext
```js
const MyContext = React.createContext(defaultValue);
```

创建一个 Context对象。当 React 渲染了一个订阅这个对象的组件，这个组件会从距离这个组件最近的那个 Provider 中读取当前的 context 值

#### Context.Provider / Context.Consumer

```js
<MyContext.Provider value={/* 某个值 */}>
```
新建的 Context 对象会返回 Provider 和 Comsumer，Provider组件 允许 Consumer组件 订阅context 的变化。Provider 组件可以和多个 Consumer 组件右对应关系，多个Provider 也可以多层嵌套, 相同的 Provider 里层的会覆盖外层的 value。
```js
<MyContext.Consumer>
  {value => /* 基于 context 值进行渲染*/}
</MyContext.Consumer>
```
Consumer 组件里可以使用函数作为子元素来获取相对应 Provider 里的value。

当 Provider 所提供的 value 变化时，会导致内部所有的 Consumer 组件更新，新旧值的比对采用的是与 Object.is 相同的算法。需要注意的是，当 value 的值是一个对象时，Provider 所在的 render 更新也会导致 Consumer 组件重新渲染，每次生成的对象即使属性是一样的也不全等。

#### contextType / ContextHooks 
为组件设置 contextType 属性，可以在组件里不适用 Consumer 组件来获取 context 所对应的值，直接读取组件的 context 属性即可

```js
class MyClass extends React.Component {
  static ccontextType = MyContext
  render() {
    const value = this.context
    return <div>{value}</div>
  }
}
```
使用 useContext 也可以有相同的效果

```js
function Login() {
  const isLogin = useContext(MyContext)
  return isLogin ? 'login' : 'need to login'
}
```