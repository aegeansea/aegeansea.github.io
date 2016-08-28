---
title: PHP中include/require/include_once/require_once使用总结
date: 2016-08-28 18:03:57
tags: [php,引用]
---
### include
使用方法：
```php
include "test.php";
```
一般是放在流程控制的处理部分中使用，将文件内容引入。PHP程序在遇到include语句时，才将它读进来，这种方式可以把程序执行时的流程简单化，便于复用代码！
<!-- more -->
include在引入不存文件时产生一个警告且脚本还会继续执行，执行时需要引用的文件每次都要进行读取和评估，且有返回值，比如：
```php
if(FALSE) { 
    include 'test.php'; // test.php不会被引入 
} 
 
<?php
  include 'test.php';// 现在的条件是test.php不存在
  echo '标哥的技术博客'; // 仍然执行下面的代码
?>
 
$ret = include "QueryPhone.php";
if (!empty($ret)) {
    echo "文件引入成功";
} else {
    echo "文件引入失败";
}
```

### include_once
使用方法：
```php
include_once "test.php";
```

加了_once之后，表示文件已引入的将不再引入。include_once引入文件的时候，如果碰到错误会给出提示并继续运行下边的代码！

他的使用方式与include差不多，不同的是include_once只引入一篇！

### require
使用方法：
```php
require "test.php";
```
一般是放在PHP文件的最前面将文件内容引入，PHP会将require的文件内容先引入成为当前文件的一部分，然后才开始执行后面的代码。

require在引入文件失败时会给出提示且脚本会被中断执行。比如：
```php
//文件是不存在的
require "QueryPhone.php";
echo "没有被打印";
```

### require_once
使用方法：
```php
require_once "test.php";
```
一般是放在PHP文件的最前面将文件内容引入，PHP会先将待引入的文件内容引入到本文件中，如果引入失败则不会继续往下执行；如果引入成功，则可正常执行下面的代码。

它的使用方式与require差不多，不同的是require_once只会引入一次，如果之前已引入过，则不会再引入！

### 综合例子
假设有一个文件中的a.php,里面只有一句echo "file name is a";
```php
<?php
include 'a.php';
require 'a.php';
 
include_once 'a.php';
require_once 'a.php';
```
那么上面这四个引入会打印多少行呢?其实只会打印前面两句代码执行的结果，因此只有两个：
```php
file name is a
file name is a
```

下面我们来交换一下前两行与后两行试试：
``` 
<?php
 
include_once 'a.php';
require_once 'a.php';
 
include 'a.php';
require 'a.php';
```

那么上面这四句会打印出多少行呢？自然是四行，因为前两行在此之前并没有引入过，因此会引入一次，而include/require虽然之前引入过，还会再引入，因此打印结果：

```php
file name is a
file name is a
file name is a
file name is a
```

### 注意事项
从理论上说，include和require后面加不加括号对执行的结果并没有什么区别，但是加上括号效率相对会较低，所以通常后面能不加括号就不要添加括号了！

### 参考
[PHP中include/require/include_once/require_once使用总结](http://www.huangyibiao.com/archives/1429)
