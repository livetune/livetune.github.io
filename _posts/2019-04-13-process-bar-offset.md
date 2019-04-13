---
layout: post
title: 一个简单的进度条
categories: html,css,javascript
description: 使用js+css变量完成一个简易的进度条
keywords: css变量,js
---

# 前言

在做张鑫旭的每周小测的时候有这样的一题，给出了进度条的基本样式，然后要完成一个可拖拽可点击的进度条。我的想法是先通过 js 监听 DOM 的变化，再改变 css 的变量来改变进度条的进度。

### DEMO

```html
<div class="slider">
  <button class="slider-track"></button>
  <button class="slider-thumb"></button>
</div>
```

```css
.slider {
  padding: 5px 0;
  position: relative;
  margin: 30px 10%;
  --percent: 0;
}
.slider-track {
  display: block;
  width: 100%;
  height: 6px;
  background-color: lightgray;
  border: 0;
  padding: 0;
}
// 进度条高亮部分
.slider-track::before {
  content: '';
  display: block;
  height: 100%;
  background-color: skyblue;
  width: calc(1% * var(--percent));
}
// 可点击的按钮
.slider-thumb {
  position: absolute;
  width: 16px;
  height: 16px;
  border: 0;
  padding: 0;
  background: #fff;
  box-shadow: 0 0 0 1px skyblue;
  border-radius: 50%;
  left: calc(1% * var(--percent));
  top: 0;
  margin: auto -8px;
}
```

### 实现

1. 首先先实现通过 js 改变进度条标签 css 的变量
   css 的变量只需要在前面加个--就可以使用了
   标签的 style.setProperty('--percent', 100) 可以直接设置变量值
2. 点击的使用只用监听最外层的 slider 标签即可

   ```js
   document.querySelector('.slider').addEventListener('click', function(e) {
     this.style.setProperty('--percent', (e.offsetX / this.clientWidth) * 100)
   })
   ```

   获取鼠标指针相对于 DOM 节点的 X 坐标，可以获取 offsetX 的属性， 同时还需要阻止 thumb 的冒泡事件，thumb 会导致点击事件中的 target 不为 slider，导致定位 offsetX 变成相对于 thumb，然后再除去进度条本身的长度即可计算出当前进度条的百分比值
   Mouse​Event​.offsetX 鼠标指针相对于目标节点内边位置的 X 坐标
   MouseEvent.pageX 鼠标指针相对于整个文档的 X 坐标
   MouseEvent.screenX 鼠标指针相对于全局（屏幕）的 X 坐标
   MouseEvent.clientX 它提供事件发生时的应用客户端区域的水平坐标

3. 拖动。
   可以不用处理 thumb 的拖动，还是只用监听 slider 的 mousedown 事件，当 mousedown 事件触发时需要为 slider 添加 mousover，来事实计算进度条的位置，当鼠标抬起时也需要将 mouseover 事件移除。

   ```js
   const dragHandle = function(e1) {
     // e1.clientX 指针在页面中的X坐标
     // $('.slider').getBoundingClientRect().x slider 相对于页面的X坐标
     var Length = e1.clientX - $('.slider').getBoundingClientRect().x
     // 边界判断
     Length <= 0 && $('.slider').style.setProperty('--percent', 0)
     Length >= $('.slider').clientWidth &&
       $('.slider').style.setProperty('--percent', 100)
     Length >= 0 &&
       Length <= $('.slider').clientWidth &&
       $('.slider').style.setProperty(
         '--percent',
         (Length / $('.slider').clientWidth) * 100,
       )
   }
   $('.slider').addEventListener('mousedown', function(e) {
     document.addEventListener('mousemove', dragHandle)
   })
   document.addEventListener('mouseup', function(e) {
     document.removeEventListener('mousemove', dragHandle)
   })
   ```

   在 slider 按下鼠标后是需要监听 document 的 mouseover 事件，如果只监听 slider 的，在鼠标离开了 slider 元素后事件就不会触发了，mouseup 同理。
   由于这次的事件不是在 slider 标签中国触发的，所以进度条高亮的位置不能像点击事件里那样进行计算。
   首先要获得当前指针在页面上的位置，然后再减去 slider 相对于视窗的 X 值即可，原本是想使用 slider 的 offsetLeft 来进行计算的，但是 offsetLeft 不会讲 margin 的长度计算进去，会出现 bug。
   同时在边界判断的时候需要考虑超过边界时的情况，如果超出边界也要设置边界值，不能不进行计算。
