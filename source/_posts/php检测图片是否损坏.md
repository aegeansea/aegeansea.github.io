---
title: php检测图片是否损坏
date: 2016-06-15 14:48:29
tags:
    - 图片
    - 损坏
categories: php
---
 坚决的使用PHP图片损坏检测,检测图片是否完整,图片是否损坏,检查本地图片是否正常 是否正确的图片：
```php
if( @imagecreatefromjpeg( $yourfile ) == false ) {
    // image is bad....http://my.oschina.net/cart/
}
```
