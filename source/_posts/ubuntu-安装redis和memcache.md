---
title: ubuntu 安装redis和memcache
date: 2018-06-21 19:46:08
tags:
---
## 安装Redis及启动
  安装Redis
  ```shell
    apt-get install redis-server
  ```
  启动服务
  ```shell
    /etc/init.d/redis-server start
  ```
 连接服务
  ```shell
    redis-cli
  ```
  查看redis的密码
  ```shell
  config get requirepass 
  ```
  设置redis密码
  ```shell
  config set  requirepass 12345
  ```
  进入redis
  ```shell
  redis-cli -h 127.0.0.1 -p 6379 -a 12345
  ```
## php安装redis扩展
1.下载phpredis扩展文件

如果服务器没有安装git服务，下载之前，首先安装一下git服务，这里不做描述，自行百度
```shell
git clone https://github.com/phpredis/phpredis.git
```
2.移动文件
```shell
mv phpredis(此处是你clone下的文件) /etc/phpredis
```
3.安装
```shell
cd /etc/phpredis
phpize
```
如果phpize命令没有响应，可能是没有安装php-dev。我目前安装的是php7.0，键入命令
```
apt-get install php7.0-dev
```
然后再phpize

4.编译

依次键入命令
```
./configure
make
make install
```
5.修改配置文件
```shell
vim /etc/php/7.0/apache2/php.ini
/php_shmop（查找字符串）
```
然后添加一行
```shell
extension=/etc/phpredis/modules/redis.so
```
6.重启apache2
```
/etc/init.d/apache2 restart
```

检查是否安装

写个phpinfo.php文件
```shell
<?php
     phpinfo();
?>
```
访问phpinfo.php，看看是否有redis模块