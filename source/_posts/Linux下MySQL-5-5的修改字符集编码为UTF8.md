---
title: Linux下MySQL_5.5的修改字符集编码为UTF8
date: 2016-08-28 18:28:43
tags: [mysql,utf8]
---
### 背景
mysql安装后, 插入的数据位乱码, 经检查为默认字符集是`latin1`, 而程序使用`UTF-8`.
<!-- more -->
### 检查字符集
1.使用ssh终端连接mysql
```bash
mysql -h 127.0.0.1 -u root -p
```

2.查看字符集
```bash
show variables like 'character%';
```

显示如下
```bash
+--------------------------+----------------------------+
| Variable_name | Value |
+--------------------------+----------------------------+
| character_set_client | utf8 |
| character_set_connection | utf8 |
| character_set_database | latin1 |
| character_set_filesystem | binary |
| character_set_results | utf8 |
| character_set_server | latin1 |
| character_set_system | utf8 |
| character_sets_dir | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
```
`character_set_database`和`character_set_server`的默认字符集还是`latin1`.


### 解决方案

修改mysql的`my.cnf`文件中的字符集键值, `my.cnf`一般位于`/etc`目录下, 没找到的话可以使用`find / -name my.cnf`查找.

1.在[client]字段里加入default-character-set=utf8
```bash
[client]
port = 3306
socket = /var/lib/mysql/mysql.sock
default-character-set=utf8
```
2.在[mysqld]字段里加入character-set-server=utf8
```bash
[mysqld]
port = 3306
socket = /var/lib/mysql/mysql.sock
character-set-server=utf8
```
3.在[mysql]字段里加入default-character-set=utf8
```bash
[mysql]
no-auto-rehash
default-character-set=utf8
```
修改完成后，service mysql restart重启mysql服务就生效.

*注意*: [mysqld]字段与[mysql]字段是有区别的.

4.再次查看字符集
```bash
show variables like 'character%';
```
如下所示：
```bash
+--------------------------+----------------------------+
| Variable_name | Value |
+--------------------------+----------------------------+
| character_set_client | utf8 |
| character_set_connection | utf8 |
| character_set_database | utf8 |
| character_set_filesystem | binary |
| character_set_results | utf8 |
| character_set_server | utf8 |
| character_set_system | utf8 |
| character_sets_dir | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
```
5.如果上面的都修改了还乱码,那剩下问题就一定在connection连接层上.
```bash
SET NAMES 'utf8';
```
该语句相当于
```bash
SET character_set_client = utf8;
SET character_set_results = utf8;
SET character_set_connection = utf8;
```

### 参考
[Linux下MySQL_5.5的修改字符集编码为UTF8](http://arccode.net/2015/07/07/Linux%E4%B8%8BMySQL-5-5%E7%9A%84%E4%BF%AE%E6%94%B9%E5%AD%97%E7%AC%A6%E9%9B%86%E7%BC%96%E7%A0%81%E4%B8%BAUTF8/#more)
