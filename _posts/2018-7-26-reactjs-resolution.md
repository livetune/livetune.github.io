---
layout: post
title: 记录一些react初始化遇到的问题
categories: react
description: 记录一些react初始化遇到的问题
keywords: 语法,javascript
---

### 使用 sass

```js
        // npm install node-sass sass-loader --save
        // webpack.config.dev.js
         {
            test: /\.s?css$/, // 正则加入 s? 匹配 scss 文件
            use: [
              require.resolve('style-loader'),
              {
                loader: require.resolve('css-loader'),
                options: {
                  importLoaders: 1,
                },
              },
              {loader: require.resolve('sass-loader')}
            ],
          },
```

### react-hot-loader

因为 react-hot-loader 的热更新是依赖于 webpack-dev-server，后者是在打包文件改变时更新打包文件或者 reload 刷新整个页面，而前者会根据 stateNode 节点的更新对比，只更新改变的 reactDom 节点，从而保留了未改变的 state 值，更适用于 react 的开发更新模式。
需要添加如下设置:

```js
// webpack.config.js
entry: [
  require.resolve("./polyfills"),
  "react-hot-loader/patch" //添加此处
];
// .babelrc如果有的话
{
  plugins: ["react-hot-loader/babel"];
}
```

### 关于路由 history

在几份源码中，他们路由使用的 BrowserHistory 来自不同的对象，主要原因应该是 react-router 的版本不同,在 v4 中不提供 BowserHistory 等方法的导出。一般还是使用 history 包的 createBrowserHistory 来创建一个 history 对象。

[https://github.com/brickspert/blog/issues/3](https://github.com/brickspert/blog/issues/3)

### 关于 express 与 koa 使用异步路由

express 使用 CPS 风格（就是异步回调），而 koa2 是
promise，async/await 的编码风格。所以在 express 中使用 mongoose 进行查询可以使用回调函数，而 koa2 中则需要加上 async 关键字使用 await 使查询变为同步。

### redux-thunk 等中间件

```js
\\ redux-thunk 源码
function createThunkMiddleware(extraArgument) {
 return ({ dispatch, getState }) => next => action => {
   if (typeof action === 'function') {
     return action(dispatch, getState, extraArgument);
   }

   return next(action);
 };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

```js
\\ applyMiddleware

export default function applyMiddleware(...middlewares) {
  return createStore => (...args) => {
    const store = createStore(...args)
    let dispatch = () => {
      throw new Error(
        `Dispatching while constructing your middleware is not allowed. ` +
          `Other middleware would not be applied to this dispatch.`
      )
    }

    const middlewareAPI = {
      getState: store.getState,
      dispatch: (...args) => dispatch(...args)
    }
    const chain = middlewares.map(middleware => middleware(middlewareAPI))
    dispatch = compose(...chain)(store.dispatch)

    return {
      ...store,
      dispatch
    }
  }
}
```

### 在使用 connect 连接容器组件和展示组件时，如果 mapDispatchToProps 参数填的是对象，将会自动使用 bindActionCreator

```js
//  mapDispatchToProps.js
export function whenMapDispatchToPropsIsObject(mapDispatchToProps) {
  return mapDispatchToProps && typeof mapDispatchToProps === "object"
    ? wrapMapToPropsConstant(dispatch =>
        bindActionCreators(mapDispatchToProps, dispatch)
      )
    : undefined;
}

export function addToCart({ id, price, count }) {
  if (!Number.isInteger(count)) {
    count = 0;
  }
  count = count > 99 ? 99 : count;
  count = count < 0 ? 0 : count;
  return { type: ADD_TO_CART, payload: { id, price, count } };
}

@connect(
  state => ({ goods: state.get("goods") }),
  {
    addToCart
  }
)
class Goods extends Component {
  render() {
    //...
  }
}
```
