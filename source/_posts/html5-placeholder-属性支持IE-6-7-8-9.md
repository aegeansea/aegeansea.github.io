---
title: html5 placeholder 属性支持IE 6 7 8 9
date: 2016-07-20 14:55:56
tags: [placeholder, js]
---
兼容IE js代码实现html5 placeholder
<!-- more -->
```javascript
/*
 * html5 placeholder, fix for IE6,7,8,9
 * @修改者  zxs
 */
var JPlaceHolder = {
    //检测浏览器是否支持 placeholder
    _check : function(){
        return 'placeholder' in document.createElement('input');
    },
    //初始化
    init : function(){
        if(!this._check()){
            this.fix();
        }
    },
    //修复
    fix : function(){
        jQuery(':input[placeholder]').each(function(index, element) {
            var self = $(this), txt = self.attr('placeholder');
            self.wrap($('<div></div>').css({position:'relative', zoom:'1', border:'none', background:'none', padding:'none', margin:'none'}));
            var pos = self.position(), h = self.outerHeight(true), paddingleft = self.css('padding-left');
            var holder = $('<span></span>').text(txt).css({position:'absolute', left:pos.left, top:'5px', height:h, lienHeight:h, paddingLeft:paddingleft,color:'#aaa'}).appendTo(self.parent());
            self.focusin(function(e) {
                holder.hide();
            }).focusout(function(e) {
                if(!self.val()){
                    holder.show();
                }
            });
            holder.click(function(e) {
                holder.hide();
                self.focus();
            });
        });
    }
};
//执行
jQuery(function(){
    JPlaceHolder.init();   
});
```

### 参考
[html5 placeholder 属性支持IE 6 7 8 9](http://www.oschina.net/code/snippet_1444646_44887)
