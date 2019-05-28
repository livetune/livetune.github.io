---
layout: post
title: 使用正则来获取 cookie 的值
categories: js,正则表达式
description: 通过正则表达式的组匹配来获取 cookie 的值
keywords: JavaScript, 正则表达式
---

在浏览器里设置 cookie 只用对 document.cookie 进行赋值，但是获取一个 cookie 的值却没有相对应的方法，一般获取 cookie 有两种方式，通过字符串分割和正则表达式，这里就直接使用正则来实现了

### DEMO

```js
// 获取一个名字为 userid 的 cookie 值

document.cookie.replace(/(?:(?:^|.*;\s*)userid\s*\=\s*([^;]*).*$)|^.*$/, '$1')

// (?:^|.*;\s*)userid 表示匹配userid开头，或者是userid前有分号的字符串，
// 不加 ?: 的话 $1 里的内容则是括号里的内容

//  userid\s*\=\s*([^;]*).*$)
//  \s*\=\s* 匹配userid之后的等号
//  ([^;]*) 匹配后面所有不是分号的字符，分号后的字符使用了 .*$匹配
//  最后的 |^.*$ 则匹配不符合 key = value 这种规则的字符串
// 匹配不到userid的话 $1 则没有内容,replace 也就返回了一个空字符串
```

### 实现

1. 字符串的 replace 可以获得使用正则的组匹配中匹配到的值。

2. 在正则表达式中使用小括号可以表示组匹配，而在括号内加上 ?: 则表示这个括号内值的为非匹配组，括号内的值不会被匹配到。

### 函数 getCookie

```js
function getCookie(str) {
  return document.cookie.replace(
    new RegExp(`(?:(?:^|.*;\\s*)${str}\\s*\\=\\s*([^;]*).*$)|^.*$`),
    '$1',
  )
}
```
