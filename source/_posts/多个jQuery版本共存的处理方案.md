---
title: 多个jQuery版本共存的处理方案
date: 2016-05-06 06:57:27
tags:
    - jQuery
    - 多版本
    - 共存
---
```html
   <script src="jquery-1.5.js"></script>
   <script src="jquery-1.11.js"></script>
   <script>
      // 现在window.$和window.jQuery是1.11版本:
      console.log($().jquery); // => '1.11.0'
      var $jq = jQuery.noConflict(true);
      // 现在window.$和window.jQuery被恢复成1.5版本:
      console.log($().jquery); // => '1.5.0'
      // 可以通过$jq访问1.11版本的jQuery了
   </script>
   <script src="myscript.js"></script>
```

在myscript.js中，用$jq就可以访问1.11版本的jQuery了。

推荐在脚本内部自带闭包方式的jQuery，使用`var $jq = $.noConflict(true);`将jQuery导出到局部变量使用。
```js
   // myscript.js
   (function () {
      // BEGIN
      /*! jQuery v1.11.1 */
      !function(a,b){"object"==typeof module&&"object"==typeof module.exports?...
      if(k&&j[k]&&(e||j[k].data)||void 0!==d||"string"!=typeof b)return k||(k=...
      },cur:function(){var a=Zb.propHooks[this.prop];return a&&a.get?a.get(thi...
      var $ = jQuery.noConflict(true);
      // TODO: javascript code here...
      // END
   })();
```
注意到$是一个局部变量，在后面的代码中，可以随时引用这个$，跟页面上其他版本的jQuery全局变量$不是一个对象。