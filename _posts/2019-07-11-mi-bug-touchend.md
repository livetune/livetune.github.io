---
layout: post
title: 记录一个小米 webview，touchend 事件失效的bug
categories: [bug]
description:  在移动端上使用轮播组件时，小米的webview上的touchend事件失效
keywords: bug
---

今天在看一个之前的旧项目时，发现了一个很奇怪的 bug。项目是一个年度总结的展示类项目，整个页面是一个垂直的轮播图，向下滑动切换下一页。  
当我在页面上按住超过一秒以后，组件没有进行切换，就直接停在那里。

![stop.jpg](https://i.loli.net/2019/07/11/5d27348563fb631910.jpg)

### 解决方案

网上查了一下，似乎是小米的 webview 似乎是在长按的时候不调用 touchend 事件，导致轮播组件不会再手指抬起时回到上一页或者下一页。解决方案是使用 touchcancel 替代 touchend 事件。touchend 是手指离开屏幕时触发的时间，touchcancel 是触控点被特定的方式打断的时候会触发的时间。

### 实现
```js
// 直接替代 touchend 时间
document.querySelector('.target').addEventListener('touchcancel',e=>{
    touchendFunc()
})

// 如果无法替代，则在 touchcancel 模拟一个touchend事件 
document.querySelector('.target').addEventListener('touchcancel',e=>{
    const event = document.createEvent('Events')
    event.initEvent('touchstart', true, true);
    document.querySelector('.target').dispatchEvent(event)
})

```