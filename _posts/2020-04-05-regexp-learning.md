---  
layout: post  
title: 正则笔记
categories: [regexp,example] 
description:  学习正则
keywords: regexp  
---  

### 贪婪与非贪婪

贪婪：尽可能匹配更多的字符
`+ {m,n} *` 重复一次或者多次

非贪婪：尽可能少的去匹配
`?` 可能有也可能没有
```javascript
// 贪婪模式会直接匹配第一个 a 到最后一个 a，无视中间出现的 a
// 非贪婪模式则是匹配到第二次出现的 a 就停止了
('abbbaaccca').match(/a.*a/g) // ['abbbaaccca']
('abbbaaccca').match(/a.*?a/g) // ['abbba','accca']
```

### 反向引用
表达式在匹配时，会将小括号里匹配到的字符串记录下来。获取引用的方法是使用 `\` 加上数字来代表第几次引用。

```javascript
// 用于匹配前后出现相同的字符比较方便，如引号等。
'"hello", \'world\''.match(/('|").*\1/g) // [ '"hello"', "'world'" ]

// [ '<span>hello </span>', '<p><span>world</span></p>' ]
'<span>hello </span><p><span>world</span></p>'.match(/<(\w+)>.*?<\/\1>/g) 
```
### 预搜索、反向预搜索

正向预搜索
`/y(?=xxxx)/` 匹配后面是 xxxx 的 y 

```javascript
'iPhone X'.match(/iPhone(?= X)/g) // ['iPhone'] 
```
`/y(?!xxxx)/` 匹配后面不是 xxxx 的 y
```javascript
'dog,done'.replace(/(do(?!g))/g,'-') //'dog,-ne' 
```

反向预搜索（后行断言）  
`/(?<=xxxx)y/` 匹配前面是 xxxx 的 y 位置
```javascript
'user:1024 free:2048'.match(/(?<=user:)(\d+)/g) // ['1024']
```

`/(?<=xxxx)y/` 匹配前面不是 xxxx 的 y 位置 

```javascript
'1234.5678'.match(/(?<!\.\d*)\d+/g) // 1234 11 22
// 小数部分不会被匹配
```