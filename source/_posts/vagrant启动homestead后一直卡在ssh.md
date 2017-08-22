---
title: vagrant启动homestead后一直卡在ssh
date: 2017-08-22 16:43:01
tags:
    - vagrant
    - homestead
categories: php
---

- 将以下代码加入Vagrantfile中

```
config.vm.provider "virtualbox" do |vb|
    vb.gui = true
 end
```

- 在virtualbox中看是否有个网路连接未启动，启动它