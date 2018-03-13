---
title: mysql
mathjax: true
date: 2018-03-13 23:19:50
tags: mysql
categories: web
---
## 1.编码
1.查看数据库编码
```
show variables like 'char%';
```
修改编码格式:
```
set character_set_database = utf8;
 set character_set_server = utf8;
```
2.查看建库语句
```
show create database your_database_name;
```
更改编码:
```
alter database `file` default character set utf8 collate utf8_general_ci;
```
> 在mysql中反引号 `` 用于用于用户自定义的量，而单引号'' 用于系统变量

3.修改数据表编码
```
alter table `file` default character set utf8 collate utf8_general_ci;
```
4.查看表字段并修改编码
 ```
show full columns from `user`
 ```
 修改编码：
 ```
 alter table `user` change name name varchar(32) character set utf8 collate utf8_general_ci;
 ```
 ## 2.修改表结构
 1.修改表名
 ```
 ALTER  TABLE admin_user RENAME TO a_user
 ```
 ## 3.数据
 1.清空数据
 ```
 truncate user;
 ```