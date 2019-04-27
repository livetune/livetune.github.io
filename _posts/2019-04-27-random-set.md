---
layout: post
title: 生成一个随机的数组
categories: js
description: 基于一个唯一的字符或数组组成的字符串生成一个随机的数组
keywords: JavaScript, 算法
---

记录下张鑫旭大佬的每周小测，这次的题目是基于一个唯一的字符或数组组成的字符串生成一个随机的数组，具体的描述如下
![github-js-quiz31.jpg](https://camo.githubusercontent.com/53b90ce367224a1a15d56ac493d2804606c6d4ad/68747470733a2f2f71696469616e2e717069632e636e2f71696469616e5f636f6d6d6f6e2f3334393537332f34663938353134326537333562306235326466333732633963333363396265302f30)

### DEMO

```js
function getRandom(userID) {
  var skins = [
    { skin: 1 },
    { skin: 2 },
    { skin: 3 },
    { skin: 4 },
    { skin: 5 },
    { skin: 6 },
    { skin: 7 },
    { skin: 8 },
    { skin: 9 },
    { skin: 10 },
  ]
  let userId = (userID + '').replace(/\D/g, matchs => {
    return matchs.charCodeAt()
  })
  let seed = (Math.abs(Math.sin(userId)) + '').substr(3, skins.length)
  let res = skins
    .map((v, index) => {
      v.order = +seed[index]
      return v
    })
    .sort((va, vb) => va.order - vb.order)
  return res
}
console.dir(getRandom('abc001'))
console.dir(getRandom('abc001'))
console.dir(getRandom('abc002'))
console.dir(getRandom('abc003'))
```

### 实现

1. 不能使用 sort 加上 Math.random 去直接生成一个随机的数组，Math.random 的随机数是符合正态分布的（两边大中间小）。但是可以给数组里的每一个元素都生成一个随机数放在 order 里，再通过 order 进行排序，这样出来的结果也是一个随机的数组。
2. 可以使用 userId 来生成一个随机的标识，通过那个标识来排序，这样以后如果改变数组里的个数，也可以保证它的随机性。可以通过种子随机数生成也可以使用正弦余弦（Math.sin、Math.cos）等函数来生成一段随机的数值。再将这些数值根据索引放入 order 里进行排序来完成随机数组。

[逼乎链接](https://www.zhihu.com/question/22818104)

```js
// 种子随机数
function rnd(seed) {
  seed = (seed * 9301 + 49297) % 233280 //为何使用这三个数?
  return seed / 233280.0
}
```

[code pen DEMO](https://codepen.io/livetune/pen/dLvzww)
