---  
layout: post  
title: React Hooks的一些学习笔记（1）
categories: [notes,react]  
description:  React Hooks的一些学习笔记  
keywords: notes,react  
---  

React 的 Hooks 在18年就已经出来了，但是由于项目中使用的版本还是比较老的 React，也就一直没有去学习。这个笔记是写给自己看的，未来如果有切入到需要使用 Hooks 的地方也好查看。
## 简介
Hooks 是 React 16.8 以后的版本才支持的特性。可以让我们在不使用 class Api 的同时访问 state 以及其他特性。

### 动机

#### 在组件之间复用状态逻辑很难
React 没有提供直接将可复用性行为“附加"到组件中的途径（直接将组件与 store 关联起来）。大量的使用 Render Prop（将一个渲染函数当成 props 传入组件的一个方法）或者是 HOC （高阶组件）会使 React 的在 React Devtool 里的层级出现嵌套地狱（多层嵌套）。

#### 复杂组件变得难以理解
我们在开发过程中，组件起初很简单，但随着项目的越来越复杂，组件里的逻辑被分散到各个生命周期里。
通常是在 cDM 里注册一个监听函数，然后在 cDUp 里更新，在 cWUn 里把监听函数卸载。独立的逻辑被分散到了不同的生命周期里。

#### 难以理解的 class  
你需要去理解在 JavaScript 中 this 的工作方式，还不能忘记绑定事件处理器。在 React 中可以使用 class 组件与函数组件，也是需要去区分这两种的使用方式。

## 概览

### State Hook

我们可以在函数组件里使用 useState 来创建一个 state。React 在重复渲染时会保留这个state，与 class 组件中的 state 一样。useState的参数是 state 的默认值，返回的第一个值的是 state，第二个则是对这个 state 进行更新的函数，可以在事件处理或者是其他地方调用。
```js
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 “count” 的 state 变量。
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

当然也可以在组件里多次创建不同的 state 。
```js
function ExampleWithManyStates() {
  // 声明多个 state 变量！
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```

### Effect Hook

我们可以把在组件中执行一些数据更新、订阅或者手动修改 Dom 的操作叫做“副作用”，也可以简称叫“作用”。  
useEffect 就是一个Effect Hook，给了函数组件操作副作用的能力。与 class 的 cDM、cDUp、cWUn 具有相同的用途，只不过是被合成了同一个Api。
```js
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // 相当于 componentDidMount 和 componentDidUpdate:
  useEffect(() => {
    // 使用浏览器的 API 更新页面标题
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
通常在 useEffect 中返回一个组件销毁时需要执行的函数, useEffect 的第二个参数是一个数组，可以规定是否需要重新执行副作用。

```js
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  },[props.friend.id]);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```
