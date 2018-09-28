---
title: mysql问题拾遗
date: 2018-09-28 23:41:44
tags:
categories:
---
## ERROR 1698 (28000) 错误
错误详情：密码正确时用户依旧无法经如数据库中
```shell
mysql -u root -p
Enter password: 
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
``` 
错误的起因就是root的plugin设置为[auth_socket](https://dev.mysql.com/doc/dev/mysql-server/latest/auth__socket_8cc.html)，用密码登陆的plugin应该是mysql_native_password。
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