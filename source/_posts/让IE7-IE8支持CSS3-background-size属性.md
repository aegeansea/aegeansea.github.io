---
title: 让IE7 IE8支持CSS3 background-size属性
date: 2016-07-20 14:59:30
tags: [css, 背景图片缩放]
---
{% asset_img 01.jpg %}
<!-- more -->
### 简介
CSS3 新增的 background-size 是一个很有用的属性，用于定义背景图片的尺寸，有了这个属性，你就可以任意指定背景图片的大小。其中最常用的值应该要数 cover 了，该值能让背景图片缩放至填满整个容器，即使是图片面积小于容器面积。

由于 background-size 是 CSS3 新增的属性，所以 IE 低版本自然就不支持了，但是老外写了一个 htc 文件，名叫 background-size polyfill，使用该文件能够让 IE7、IE8 支持 background-size 属性。其原理是创建一个 img 元素插入到容器中，并重新计算宽度、高度、left、top 等值，模拟 background-size 的效果。

### 使用方法
直接在样式中写入即可，如：
```css
body {
    height: 100%;
    margin: 0;
    background: url(images/126.jpg) center no-repeat;
    background-size: cover;
    -ms-behavior: url(backgroundsize.min.htc);
    behavior: url(backgroundsize.min.htc);
}
```

### 局限性
background-size polyfill 虽然可以模拟 background-size 属性，但并不能完全模拟，毕竟 background 方式和 img 方式还是有区别的，主要的支持情况如下：

支持
* 背景图像的正确位置和大小
* 浏览器缩放时及时更新
* 更新图片（替换等）时及时更新

不支持
* 多个背景（多重背景）
* 4 个值的 background-position
* 背景重复
* 非默认值的 background-[clip/origin/attachment/scroll]

由于 background-size polyfill 需要进过复杂的计算，所以可能会出现图片“一闪”的情况。并且 .htc 文件还不能跨域，使用 CDN 的需要注意。

虽然 background-size polyfill 有一定的局限性，但总比没有好，在某些情况下还是一个很好的选择。

 ### 参考
[让IE7 IE8支持CSS3 background-size属性](http://www.dowebok.com/139.html)
[百度云参考代码](http://pan.baidu.com/s/1miOxCJu "让IE7 IE8支持CSS3 background-size属性.7z") zpvl
