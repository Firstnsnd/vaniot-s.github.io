---
title: mysql问题拾遗
date: 2018-09-28 23:41:44
tags: mysql
categories: web
---
## ERROR 1698 (28000) 错误
错误详情：密码正确时用户依旧无法经如数据库中
```shell
mysql -u root -p
Enter password: 
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
```
错误的起因是root的plugin设置为[auth_socket](https://dev.mysql.com/doc/dev/mysql-server/latest/auth__socket_8cc.html)，用密码登陆的plugin应该是mysql_native_password。
> 解决办法:

在`/etc/mysql/mysql.conf.d/mysqld.cnf`这个文件里找到`[mysqld]`在该配置项下添加 `skip-grant-tables `这个配置,之后可以使用`mysql`命令直接进入而不需输入密码。

```shell
select user, plugin from mysql.user;
+-----------+-----------------------+
| user      | plugin                |
+-----------+-----------------------+
| root      | auth_socket           |
| mysql.sys | mysql_native_password |
| dev       | mysql_native_password |
+-----------+-----------------------+
```