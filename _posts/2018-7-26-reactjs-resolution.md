---
layout: post
title: 记录一些react初始化遇到的问题
categories: interview
description: 记录一些react初始化遇到的问题
keywords: 语法,javascript
---

### react-hot-loader

因为react-hot-loader的热更新是依赖于webpack-dev-server，后者是在打包文件改变时更新打包文件或者reload刷新整个页面，而前者会根据stateNode节点的更新对比，只更新改变的reactDom节点，从而保留了未改变的state值，更适用于react的开发更新模式。
需要添加如下设置:

```js
// webpack.config.js 
entry: [
    require.resolve('./polyfills'),
    'react-hot-loader/patch', //添加此处
]
// .babelrc如果有的话
  {
        plugins: ["react-hot-loader/babel"]
}
```

### 关于路由history 

在几份源码中，他们路由使用的history来自不同的对象，主要原因应该是react-router的版本不同,在v4中不提供bowserHistory等方法的导出。一般还是使用history包的createBrowserHistory来创建一个history对象

[https://github.com/brickspert/blog/issues/1](https://github.com/brickspert/blog/issues/1)