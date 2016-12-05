---
title: mysql操作集合
date: 2016-12-05 08:56:44
tags: [mysql,操作]
---
### 说明
mysql相关不会或容易忘记的操作
<!-- more -->
### 链接从服务器
1.不能只使用`-P`来指定端口，必须使用`.sock`来链接(原因不明)
```bash
mysql -S /home/mysql2/mysql.sock -P 3308 -uroot -pmeidi
```

### 链接从服务器
1.不能只使用`-P`来指定端口，必须使用`.sock`来链接(原因不明)
```bash
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
flush privileges;
```
#### 参考
[MySQL数据库远程连接开启方法](http://www.jb51.net/article/24508.htm)
