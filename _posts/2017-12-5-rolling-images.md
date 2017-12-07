---
layout: post
title: css循环轮播图实现
categories: [css,html,js]
description: 使用css实现无限循环的轮播图
keywords: 循环, 轮播图
---

# 原理

使用一个div来展示图片，div的宽度最好是每个图片加上图片间距的长度。然后设置图片的父容器的宽度，这里的父容器是ul。
然后就可以设置ul的translateX来控制展示的图片了。
设置动画的关键帧由0%到100%-减去container的宽度，就可以做到无缝的轮播了。

# html部分
```
<div class="container">
    <ul class="img">
        <li class="item"></li>
        <li class="item"></li>
        <li class="item"></li>
        <li class="item"></li>
        <li class="item"></li>
        <li class="item"></li>
    </ul>
</div>
```

# css部分
```
.container {
    width: 220px;
    height: 100px;
    overflow: hidden;
    padding: 0;
}

@keyframes imgroll {
    from {
        transform: translateX(0%)
    }
    to {
        transform: translateX(calc(-100% + 220px))
    }
}

.img {
    white-space: nowrap;
    padding: 0;
    font-size: 0;
    animation: imgroll linear infinite 4s;
}

.item {
    display: inline-block;
    width: 100px;
    height: 100px;
    margin-right: 10px;
}

.img li:nth-child(n) {
    background: #7d66ee
}

.img li:nth-child(2n) {
    background: #444444
}

.img li:nth-child(3n) {
    background: #96b0ed
}
```

# js
```
window.onload = function () {
    let a = document.getElementsByClassName('img')[0];
    a.style.width = 110 * 6 + 'px';
}
```
