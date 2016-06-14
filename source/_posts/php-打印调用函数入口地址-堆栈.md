---
title: php 打印调用函数入口地址(堆栈)
date: 2016-03-28 14:30:21
tags:
    - php
    - 调试
---
### 堆栈调试
``` php
function print_stack_trace() {
    $array =debug_backtrace();
    //print_r($array);//信息很齐全
    unset($array[0]);
    foreach($array as $row) {
       $html .=$row['file'].':'.$row['line'].'行,调用方法:'.$row['function']."<br>";
    }
    return $html;
}
```