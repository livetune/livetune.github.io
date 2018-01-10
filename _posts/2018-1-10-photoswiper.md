---
layout: post
title: photoswiper实现简单的图片浏览器
categories:  jquery
description: 使用jqurey插件photoswiper，实现简单的图片浏览器效果
keywords: jquery, photoswiper, 
---

## 开头
在移动端实现像微信朋友圈的功能使用原生写有稍许繁琐，需要考虑图片的双指放大以及图片的滑动，使用jquery的插件photoswiper可以快速的实现这个效果。不过相比与淘宝的三倍放大，photoswiper则是在初始化时就必须填写双击放大时的宽高，有少许不方便。

## 实现

### js以及css导入

```
    <link rel="stylesheet" href="__STATIC__/photoswiper/photoswipe.css">
    <link rel="stylesheet" href="__STATIC__/photoswiper/default-skin/default-skin.css">
    <script src="__STATIC__/photoswiper/photoswipe.js"></script>
    <script src="__STATIC__/photoswiper/photoswipe-ui-default.min.js"></script>
```

### html 

```
<div class="pswp" tabindex="-1" role="dialog" aria-hidden="true">

    <!-- Background of PhotoSwipe.
         It's a separate element, as animating opacity is faster than rgba(). -->
    <div class="pswp__bg"></div>

    <!-- Slides wrapper with overflow:hidden. -->
    <div class="pswp__scroll-wrap">

        <!-- Container that holds slides. PhotoSwipe keeps only 3 slides in DOM to save memory. -->
        <div class="pswp__container">
            <!-- don't modify these 3 pswp__item elements, data is added later on -->
            <div class="pswp__item"></div>
            <div class="pswp__item"></div>
            <div class="pswp__item"></div>
        </div>

        <!-- Default (PhotoSwipeUI_Default) interface on top of sliding area. Can be changed. -->
        <div class="pswp__ui pswp__ui--hidden">

            <div class="pswp__top-bar">

                <!--  Controls are self-explanatory. Order can be changed. -->

                <div class="pswp__counter"></div>

                <button class="pswp__button pswp__button--close" title="Close (Esc)"></button>

                <button class="pswp__button pswp__button--zoom" title="Zoom in/out"></button>

                <!-- Preloader demo https://codepen.io/dimsemenov/pen/yyBWoR -->
                <!-- element will get class pswp__preloader--active when preloader is running -->
                <div class="pswp__preloader">
                    <div class="pswp__preloader__icn">
                        <div class="pswp__preloader__cut">
                            <div class="pswp__preloader__donut"></div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="pswp__share-modal pswp__share-modal--hidden pswp__single-tap">
                <div class="pswp__share-tooltip"></div>
            </div>

            <div class="pswp__caption">
                <div class="pswp__caption__center"></div>
            </div>

        </div>

    </div>

</div>

```

### javascript

```
        var openPhotoSwipe = function(imgList, index) {
            var pswpElement = document.querySelectorAll('.pswp')[0];
            var options = {
                // history & focus options are disabled on CodePen
                history: false,
                focus: false,
                index:index,
                showAnimationDuration: 0,
                hideAnimationDuration: 0

            };

            var gallery = new PhotoSwipe( pswpElement, PhotoSwipeUI_Default, imgList, options);
            gallery.init();
        };
```

### 调用

```
        $('.comment-page-wrapper').on('click','.comment_item .img',function () {
            var imgList = [];
            $(this).parents('.img-list').find('img').each(function () {
                imgList.push({src:$(this).attr('src'),w:this.naturalWidth,h:this.naturalHeight});
            });
            openPhotoSwipe(imgList,$(this).index());
        });

```

### 效果

![微信截图_20180110152439.png](https://i.loli.net/2018/01/10/5a55bff073ddc.png)
