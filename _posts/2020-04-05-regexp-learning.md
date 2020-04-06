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
```js
// 贪婪模式会直接匹配第一个 a 到最后一个 a，无视中间出现的 a
// 非贪婪模式则是匹配到第二次出现的 a 就停止了
('abbbaaccca').match(/a.*a/g) // ['abbbaaccca']
('abbbaaccca').match(/a.*?a/g) // ['abbba','accca']
```

### 反向引用
表达式在匹配时，会将小括号里匹配到的字符串记录下来。获取引用的方法是使用 `\` 加上数字来代表第几次引用。

```js
// 用于匹配前后出现相同的字符比较方便，如引号等。
'"hello", \'world\''.match(/('|").*\1/g) // [ '"hello"', "'world'" ]

// [ '<span>hello </span>', '<p><span>world</span></p>' ]
'<span>hello </span><p><span>world</span></p>'.match(/<(\w+)>.*?<\/\1>/g) 
```
### 预搜索、反向预搜索

正向预搜索
`/(?=xxxx)/` 只匹配右侧满足 xxxx 的字符串, 自身内容不会被匹配
```js
'iPhone X'.match(/iPhone(?= X)/g) // ['iPhone'] 
```
`/(?!xxxx)/` 右侧不满足 xxxx 匹配
```js
'dog,done'.replace(/(do(?!g))/g,'-') //'dog,-ne' 
```

反向预搜索（后行断言）
`/(?<=xxxx)y/` 只有 y 在 xxxx 后面才匹配
```js
'user:1024 free:2048'.match(/\w+:(?=\d+)/g) // ['user:']
'user:1024 free:2048'.match(/(?<=user:)(\d+)/g) // ['1024']
```

`/(?<=xxxx)y/` 只有 xxxx 不在 y后面才匹配 
先找到 y 再判断是否匹配 xxxx
```js
'1234.5678 11a22'.match(/(?<!\.\d*)\d+/g) // 1234 11 22
// 找到 1234 1234前面没有 不匹配 \.\d*
// 找到 5678 5678 匹配 \.\d*
// 如果使用?!话 结果是 1234 5678 11 22 直接找到所有前面不是 \.\d* 的字符串
```