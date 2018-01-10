---
title: lnmp环境搭建
date: 2018-01-10 14:00:33
tags: [nginx,php,mysql]
categories: web
---
### 一、安装nginx
首先来安装nginx，使用如下命令：
```
sudo apt-get -y install nginx
```
查看nginx安装的文件。使用如下命令进行查看，如下：

```
dpkg -S nginx
```
nginx默认的安装位置是/etc/nginx目录，而且nginx的配置文件nginx.conf也是在该目录下。
除此之外，nginx的默认网站目录在/usr/share/nginx/html下，默认nginx网站配置文件为/etc/nginx/sites-available/目录下的default文件。
启动nginx，可以使用如下命令：
```
sudo /etc/init.d/nginx start
sudo service nginx start
```
### 二、安装php与php-fpm
nginx安装完毕后，安装php与php-fpm，使用如下命令，如下：
```
sudo apt-get -y install php7 php7-fpm php7-cli
```
### 三、nginx与php-fpm集成
nginx与php安装完毕后，开始把nginx与php集成。其实nginx与php集成是通过fastcgi来实现，而fastcgi一般使用的是php-fpm。
php-fpm与nginx通信方式有两种，一种是TCP方式，一种是unix socket方式。
TCP方式就是使用TCP端口连接，一般是127.0.0.1:9000。
Socket是使用unix domain socket连接套接字/dev/shm/php-cgi.sock(很多教程使用路径/tmp，而路径/dev/shm是个tmpfs，速度比磁盘快得多)，在服务器压力不大的情况下，tcp和socket差别不大，但是在压力比较满的时候，使用套接字方式，效果确实比较好。

3.1 TCP方式
先来修改nginx的默认网站文件default，如下：
```
sudo vi /etc/nginx/sites-available/default
location ~ \.php$ {
   fastcgi_split_path_info ^(.+\.php)(/.+)$;
   fastcgi_pass 127.0.0.1:9000;
   fastcgi_index index.php;
   include fastcgi_params;
}
修改nginx的fastcgi_params文件，添加如下命令。
注意：这个命令一定要添加，否则nginx与php集成后，网页会显示空白。
``
sudo vi /etc/nginx/fastcgi_params
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
```
修改php的配置文件php.ini，如下：
```
sudo vi /etc/php5/fpm/php.ini +758
将cgi.fix_pathinfo=1修改为 cgi.fix_pathinfo=0
```
修改php-fpm的配置文件www.conf，如下：

```
sudo vi /etc/php7/fpm/pool.d/www.conf
修改liten为
listen = 127.0.0.1:9000
```
修改完毕后，我们现在来重启nginx与php-fpm，如下：
```
sudo /etc/init.d/nginx restart
sudo /etc/init.d/php7-fpm restart
```
3.2 socket方式
修改nginx的默认网站文件default，如下：
```
sudo vi /etc/nginx/sites-available/default
location ~ \.php$ {
fastcgi_split_path_info ^(.+\.php)(/.+)$;
fastcgi_pass unix:/var/run/php7-fpm.sock;
fastcgi_index index.php;
include fastcgi_params; 
}
```
现在再来修改php-fpm的配置文件www.conf，如下：
```
sudo vi /etc/php7/fpm/pool.d/www.conf
listen = /var/run/php7-fpm.sock
```
再来重启nginx与php-fpm。如下：
```
sudo /etc/init.d/nginx restart
sudo /etc/init.d/php7-fpm restart
```
### 四、安装mysql
现在来开始安装mysql，如下：
```
sudo apt-get –y install mysql-server mysql-client php7-mysql
```
