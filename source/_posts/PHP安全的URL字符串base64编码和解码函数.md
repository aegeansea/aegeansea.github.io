---
title: PHP安全的URL字符串base64编码和解码函数
date: 2016-07-25 13:32:26
tags: [函数]
---
如果直接使用base64_encode和base64_decode方法的话，生成的字符串可能不适用URL地址。下面的方法可以解决该问题：
<!-- more -->
URL安全的字符串编码：
```php
function urlsafe_b64encode($string) {
   $data = base64_encode($string);
   $data = str_replace(array('+','/','='),array('-','_',''),$data);
   return $data;
 }
```

URL安全的字符串解码：

```php
function urlsafe_b64decode($string) {
   $data = str_replace(array('-','_'),array('+','/'),$string);
   $mod4 = strlen($data) % 4;
   if ($mod4) {
       $data .= substr('====', $mod4);
   }
   return base64_decode($data);
 }
```
 ### 参考
[PHP安全的URL字符串base64编码和解码](http://www.jb51.net/article/51239.htm)
