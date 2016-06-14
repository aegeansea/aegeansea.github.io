---
title: 地址栏插入js标签代码
date: 2016-03-28 16:09:25
tags:
    - js
---
### 一、代码模板
``` javascript
javascript:void(function(d){var src="codePath";var e=d.createElement('script');e.byebj=true;e.src=src+'&t='+( new Date());var b=d.getElementsByTagName('body')[0];b.firstChild ? b.insertBefore(e, b.firstChild) : b.appendChild(e);}(document));
```

### 二、修改src地址
``` javascript
src='http://code.jquery.com/jquery-2.0.3.js'
```

### 三、替换空格
将空格替换为 %20