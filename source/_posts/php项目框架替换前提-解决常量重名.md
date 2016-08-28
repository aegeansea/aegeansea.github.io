---
title: php项目框架替换前提-解决常量重名
date: 2016-08-22 10:03:48
tags: [php, 框架替换, 常量定义查询]
---
### 一、在前项目入口文件第一行
```php
$defined_constants_arr = get_defined_constants();
```
### 二、在前项目页面输出结束前
```php
global $defined_constants_arr;
echo "<pre>";
print_r(array_diff(get_defined_constants(), $defined_constants_arr));
```
