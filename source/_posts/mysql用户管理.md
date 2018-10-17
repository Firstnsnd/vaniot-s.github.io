---
title: mysql用户管理
date: 2018-04-09 09:47:41
tags:
---
创建用户并设置密码：
```mysql
create user test IDENTIFIED BY '1234546';
```
为新用户分配权限:
```mysql
//为该用户分配所有的权限
GRANT ALL PRIVILEGES ON *.* TO 'test'@'localhost' IDENTIFIED BY '123456';
//查看当前用户的权限
SHOW GRANTS;
//撤销上一次的授权
REVOKE ALL PRIVILEGES ON *.* FROM 'test'@'localhost'

```