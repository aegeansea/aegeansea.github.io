---
title: 表单提交请求导出Excel表格的实现代码
date: 2016-07-20 14:11:42
tags: [表单提交, excel导出]
---
表单提交到后台，后台返回excel表格提示下载
### 一、 代码
```javascript
    $("#ajax_form_export_submit").click(function(){
      var dataParams = {};
      var sdate = $('#sdate').val();
      var edate = $('#edate').val();
      dataParams['sdate'] = sdate;
      dataParams['edate'] = edate;
      var params = $.param(dataParams);
      var url = "{:U('exportA')}"+"&"+params;
      $('<form method="post" action="' + url + '"></form>').appendTo('body').submit().remove();
    });
```

### 参考
[Jquery ajax请求导出Excel表格的实现代码](http://www.jb51.net/article/86237.htm)
