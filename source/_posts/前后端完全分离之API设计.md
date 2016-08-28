---
title: 前后端完全分离之API设计
date: 2016-08-28 19:07:12
tags: [前后端分离,API]
---
### 背景

API就是开发者使用的界面。我的目标不仅是能用，而且好用, 跨平台(PC, Android, IOS, etc…)使用; 本文将详细介绍API的设计及异常处理, 并将异常信息进行封装友好地反馈给前端.

前后端完全分离后, 前端和后端如何交互?

答: 通过双方协商好的API.

接下来我分享我自己设计的API接口, 欢迎各位朋友指教.
<!-- more -->
### API设计理念
1. 将涉及的实体抽象成资源, 即按`id`访问资源, 在`url`上做文章, 以后再也不用为`url`起名字而苦恼了.
1. 使用`HTTP`动词对资源进行`CRUD`(增删改查); get->查, post->增, put->改, delete->删.
1. URL命名规则, 对于资源无法使用一个单数名词表示的情况, 我使用中横线(`-``)连接.
    - 资源采用名词命名, e.g: 产品 -> product
    - 新增资源, e.g: 新增产品, url -> /product , verb -> POST
    - 修改资源, e.g: 修改产品, url -> /products/{id} , verb -> PUT
    - 资源详情, e.g: 指定产品详情, url -> /products/{id} , verb -> GET
    - 删除资源, e.g: 删除产品, url -> /products/{id} , verb -> DELETE
    - 资源列表, e.g: 产品列表, url -> /products , verb -> GET
    - 资源关联关系, e.g: 收藏产品, url -> /products/{id}/star , verb -> PUT
    - 资源关联关系, e.g: 删除收藏产品, url -> /products/{id}/star , verb -> DELETE

目前我API的设计只涉及这两点, 至于第三点`HATEOAS`(Hypermedia As The Engine Of Application State)那就由读者自己去选择了.

### 项目地址

本文中只涉及了设计的理念, 具体的实现请下载源码[rest-api](https://github.com/arccode/rest-api), 项目内写了比较详细的注释.

### 参考
[前后端完全分离之API设计](http://arccode.net/2015/04/18/%E5%89%8D%E5%90%8E%E7%AB%AF%E5%AE%8C%E5%85%A8%E5%88%86%E7%A6%BB%E4%B9%8BAPI%E8%AE%BE%E8%AE%A1/)
