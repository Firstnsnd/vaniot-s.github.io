---
title: Redis(一)
date: 2018-02-04 02:45:00
tags: Redis
categories: web
---
# 安装与配置
## 下载Redis
[官方网站](http://redis.io)及[官方下载](http://redis.io/download) 根据需要下载不同版本。
>官网上没有windows版本的，如果需要在windows下安装redis的话需要到github上下载。
[windows版](https://github.com/MSOpenTech/redis/releases)

## windows下安装Redis
根据网上的教程，下载完成后解压到硬盘下比如
D:\Redis\redis，在D:\Redis\redis\bin\release下有两个zip包一个32位一个64位，根据自己windows的位数 解压到D:\Redis\redis根目录下。但是我在该目录下找不到相应的压缩文件，于是乎需要另辟蹊径。
<!--more-->
### 下载解压软件放在bin目录下(github)
[下载](https://github.com/ServiceStack/redis-windows)后解压，可根据相应的版本解压相应的压缩文件到bin目录下
文件目录：

    redis-server.exe：服务程序
    redis-check-dump.exe：本地数据库检查
    redis-check-aof.exe：更新日志检查
    redis-benchmark.exe：性能测试，用以模拟同时由N个客户端发送M个 SETs/GETs 查询 (类似于 Apache 的ab 工具).
    redis.conf　　　　配置文件
### 测试Redis运行（未成功过)
打开控制台，进入bin目录，运行redis-server.exe redis-windows.conf
![redis-run](https://github.com/Vaniot-s/picture/blob/master/redis/redis_run.png?raw=true)
### 开启Redis Client测试
打开一个新的控制台，进入bin目录，运行redis-cli.exe
![redis-run](https://raw.githubusercontent.com/Vaniot-s/picture/master/redis/_client_run.jpeg)
### 运行示例
![redis-test](https://github.com/Vaniot-s/picture/blob/master/redis/redis_test.jpeg?raw=true)

## linux安装
下载地址[](http://redis.io/download)，下载最新文档版本,下载并安装：

```
  $ wget http://download.redis.io/releases/redis-2.8.17.tar.gz
  $ tar xzf redis-2.8.17.tar.gz
  $ cd redis-2.8.17
  $ make
  $ make test //运行测试，确认Redis功能是否正常
```
make完后 redis-2.8.17目录下会出现编译后的redis服务程序redis-server,还有用于测试的客户端程序redis-cli,两个程序位于安装目录 src 目录下：

下面启动redis服务.
```
$ cd src
$ ./redis-server
```
> 这种方式启动redis 使用的是默认配置,也可以通过启动参数告诉redis使用指定配置文件使用下面命令启动。
```
$ cd src
$ ./redis-server redis.conf
```
redis.conf是一个默认的配置文件,可以根据需要使用自己的配置文件。
启动redis服务进程后，就可以使用测试客户端程序redis-cli和redis服务交互了。 比如：
```
$ cd src
$ ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```
将可执行文件放置在$PATH环境目录下，便于以后执行程序时可以不用输入完整的路径
```
$　cp redis-server /usr/local/bin/
$　cp redis-cli /usr/local/bin/
```
查看redis:
```
$  ps -ef | grep redis
```
 通过端口号检查Redis服务器状态
```
$ netstat -nlt|grep 6379
```
## Ubuntu下安装
在 Ubuntu 系统安装 Redis 可以使用以下命令:
```
$sudo apt-get update
$sudo apt-get install redis-server
```
启动 Redis
```
$ redis-server
```
查看 redis 启动
```
$ redis-cli
```
以上命令将打开以下终端：
```
redis 127.0.0.1:6379>
```
127.0.0.1 是本机 IP ，6379 是 redis 服务端口。现在我们输入 PING 命令。
```
redis 127.0.0.1:6379> ping
PONG
```
## 配置PHP-Redis的扩展
### 下载dll文件 
根据对应的php版本以及对应的电脑配置下载就行

    php_redis-5.5-vc11-ts-x86-00233a.zip http://d-h.st/4A5
    php_igbinary-5.5-vc11-ts-x86-c35d48.zip http://d-h.st/QGH
    php_redis-5.5-vc11-nts-x86-00233a.zip http://d-h.st/uGS
    php_igbinary-5.5-vc11-nts-x86-c35d48.zip http://d-h.st/bei
    php_redis-5.5-vc11-ts-x64-00233a.zip http://d-h.st/1tO（选择）  
    php_igbinary-5.5-vc11-ts-x64-c35d48.zip http://d-h.st/rYb(选择）
    php_redis-5.5-vc11-nts-x64-00233a.zip http://d-h.st/N0d
    php_igbinary-5.5-vc11-nts-x64-c35d48.zip http://d-h.st/c1a
看下自己的phpinfo的信息
![phpversion](https://github.com/Vaniot-s/picture/blob/master/redis/phpversion.png?raw=true)
![zendversion](https://github.com/Vaniot-s/picture/blob/master/redis/zendversion.png?raw=true)
就选择ts-X64的包来下载
###添加extension扩展(注意顺序)
将下载解压后的php_igbinary.dll和php_redis.dll放入php的ext目录下
然后修改php.ini，加入这两个扩展，注意顺序不要反了。
extension=php_igbinary.dll
extension=php_redis.dll
添加完之后重启WEBServer，查看phpinfo看到有redis扩展的信息就说明可以了
![redis sucess](https://github.com/Vaniot-s/picture/blob/master/redis/edis-success.png?raw=true)
### PHP-Redis扩展测试
PHP-Redis使用示例代码

    $redis = new Redis();
    $redis->connect("127.0.0.1", "6379");  //php客户端设置的ip及端口
    //存储一个值
    $redis->set("say","Hello World");
    echo $redis->get("say");     //输出Hello World
    //存储多个值
    $array = array('first_key'=>'first_val',
    'second_key'=>'second_val',
    'third_key'=>'third_val');
     $array_get=array('first_key','second_key','third_key');
     $redis->mset($array);
     var_dump($redis->mget($array_get));