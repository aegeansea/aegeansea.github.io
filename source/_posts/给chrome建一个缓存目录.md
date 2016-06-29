---
title: 给chrome建一个缓存目录
date: 2016-06-29 09:09:37
tags: [缓存]
---
- ## 第一步
删除 `C:\Users\用户名\AppData\Local\Google\Chrome\User Data\Default\` 下的 `Cache` 文件夹

- ## 第二步
``` cmd
mklink /D "C:\Users\用户名\AppData\Local\Google\Chrome\User Data\Default\Cache" "Z:\Chromecache"
```
