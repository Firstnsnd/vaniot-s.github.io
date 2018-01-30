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
sudo apt-get -y install php7.1 php7.1-fpm php7.1-cli
```
### 三、nginx与php-fpm集成
nginx与php安装完毕后，开始把nginx与php集成。其实nginx与php集成是通过fastcgi来实现，而fastcgi一般使用的是php-fpm。
php-fpm与nginx通信方式有两种，一种是TCP方式，一种是unix socket方式。

- TCP方式就是使用TCP端口连接，一般是127.0.0.1:9000。
- Socket是使用unix domain socket连接套接字/dev/shm/php-cgi.sock(很多教程使用路径/tmp，而路径/dev/shm是个tmpfs，速度比磁盘快得多)，在服务器压力不大的情况下，tcp和socket差别不大，但是在压力比较满的时候，使用套接字方式，效果确实比较好。

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
```
检验nginx的配置：
```
sudo nginx -t
```
修改nginx的fastcgi_params文件，添加如下命令。
注意：这个命令一定要添加，否则nginx与php集成后，网页会显示空白。
```
sudo vi /etc/nginx/fastcgi_params
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
```
修改php的配置文件php.ini，如下：
```
sudo vi /etc/php/7.1/fpm/php.ini +758
将cgi.fix_pathinfo=1修改为 cgi.fix_pathinfo=0
```
修改php-fpm的配置文件www.conf，如下：

```
sudo vi /etc/php/7.1/fpm/pool.d/www.conf
修改liten为
listen = 127.0.0.1:9000
```
修改完毕后，我们现在来重启nginx与php-fpm，如下：
```
sudo /etc/init.d/nginx restart
sudo /etc/init.d/php7.1-fpm restart
```
3.2 socket方式
修改nginx的默认网站文件default，如下：
```
sudo vi /etc/nginx/sites-available/default
location ~ \.php$ {
fastcgi_split_path_info ^(.+\.php)(/.+)$;
fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
fastcgi_index index.php;
include fastcgi_params; 
}
```
修改nginx的fastcgi_params文件，添加如下命令。
注意：这个命令一定要添加，否则nginx与php集成后，网页会显示空白。
```
sudo vi /etc/nginx/fastcgi_params
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
```
修改php的配置文件php.ini，如下：
```
sudo vi /etc/php/7.1/fpm/php.ini +758
将cgi.fix_pathinfo=1修改为 cgi.fix_pathinfo=0
```
现在再来修改php-fpm的配置文件www.conf，如下：
```
sudo vi /etc/php/7.1/fpm/pool.d/www.conf
listen = /var/run/php/php7.1-fpm.sock
```
再来重启nginx与php-fpm。如下：
```
sudo /etc/init.d/nginx restart
sudo /etc/init.d/php7.1-fpm restart
```
### 四、安装mysql
现在来开始安装mysql，如下：
```
sudo apt-get –y install mysql-server mysql-client php7.1-mysql
```
### 五、搭建laravel环境
#### 1.php拓展

        sudo apt-get –y install  php7.1-mcrypt //安装php7.1的加密拓展库
启用php的加密拓展库

        sudo phpenmod mcrypt
#### 2.安装 Composer
在命令行执行：

        curl -ss https://getcomposer.org/installer | php
在当前目录会发现 composer.phar 这个文件，这个文件就是 Compoesr 的执行文件，我们需要移到 /usr/local/bin , 这样全局就能调用 Composer 。

        sudo mv composer.phar /usr/local/bin/composer
Composer 安装完成。
#### 3.安装laravel
用composer来安装 Laravel

        sudo composer create-project laravel/laravel --prefer-dist
更改网站目录所属组：

        sudo chown -R :www-data /var/www/laravel
        sudo chown -R  777 storage
#### 4.配置 Nginx
开 nginx 默认配置文件：

        sudo vim /etc/nginx/sites-available/default
 增加如下配置：
 ```
server {
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;

	# 设定网站根目录
    root /var/www/laravel/public;
    # 网站默认首页
    index index.php index.html index.htm;

	# 服务器名称，server_domain_or_IP 请替换为自己设置的名称或者 IP 地址
    server_name server_domain_or_IP;

	# 修改为 Laravel 转发规则，否则PHP无法获取$_GET信息，提示404错误
    location / {
        try_files $uri $uri/ /index.php?$query_string;        
        add_header 'Access-Control-Allow-Origin' 'http://localhost:8080/';# 前后段分离时允许跨域请求
    }

	# PHP 支持
    location ~ \.php$ {
        try_files $uri /index.php =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }
}
```
修改完成，要重启下 nginx 服务：

        sudo nginx -t
        sudo service nginx restart
   在浏览器中打开localhost:
   ![laravel](https://github.com/Vaniot-s/picture/blob/master/php/lnmp/laravel.png?raw=true)