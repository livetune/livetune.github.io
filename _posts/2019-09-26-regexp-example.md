---  
layout: post  
title: 记录一些已经用过的正则表达式
categories: [regexp,example] 
description:  React Context 的使用
keywords: regexp  
---  

正则这个东西写完之后太容易被遗忘了，在这里记下使用过的正则。

### 删除除了某个特定属性值之外的属性
```javascript
const str = '<svg fill="none"><path fill="red"></path></svg>'
const newStr = str.replace(/fill\s*\=\s*"(.*?)"/g, (str, $1) => $1 !== 'none' ? '' : str).replace(/\s+(fill)\s+/,' ')
```

### 银行卡加空格

```javascript
var bankCode = '6222081812002934027'
var formatterCode = bankCode.replace(/(\d{4})(?!$)/g, '$1')
console.log(formatterCode)
```

### 数字加逗号
`(?<!\.\d*)` 不匹配小数点以后的值
```javascript
var numberCode = 1233024.1235888
var formatterNumberCode = String(numberCode).replace(/(?<!\.\d*)\B(?=(\d{3})+(?!\d))/g, ',')
console.log(formatterNumberCode)
// or 不考虑小数，简单点的
numberCode.replace(/\B(?=(\d{3})+$)/g, ',');
```