---
title: php socket(一)
date: 2018-03-27 13:12:02
tags: [socket,php]
---
### Socket函数
#### socket_create
  函数 `resource socket_create(int $domain,int $type,int $protocol)`
- domain: AF_INET、AF_INET6、AF_UNIX，\*AF (address family)地址族*。
- type: SOCK_STREAM、SOCK_DGRAM等，常用SOCK_STREAM，基于字节流的SOCKET类型，也是TCP协议使用的类型
- protocol: SOL_TCP、SOL_UDP
<!--more-->
#### socket_bind
函数: `bool socket_bind(resource $socket,string $address[,int $port=0])`
- socket: 使用 `socket_create` 创建的socket资源
- address: ip 地址
- port: 监听的端口号，web服务器通常为80端口
#### socket_listen
函数 `bool socket_listen(resource $socket[,int $bcklog=0])`
- socket: 使用 `socket_create` 创建的socket资源
- backlog: 等待处理连接队列的最大长度
#### socket_acccept
函数: `resource socket_accept(resource $socket)`
- socket: 使用`socket_create`创建的socket资源
#### socket_write
函数： `int socket_write(resource $socket,string $buffer[,int $length])`
- socket: 调用 `socket_accept` 接收的新连接产生 `socket` 资源
- buffer: 写入到 `socket` 资源中的数据
- length: 控制写入到 `socket` 资源中的 `buffer` 的长度，如果长度大于 `buffer` 的容量，则取 `buffer` 的容量。
#### socket_close
函数： `void socket_close(resource $socket)`
- socket: `socket_accept` 或者 `socket_create` 产生的资源，不能用于 `stream` 资源的关闭。
```php
//server.php
<?php
$sock=socket_create(AF_INET,SOCK_STREAM,SOL_TCP);
socket_bind($sock,"127.0.0.1",80);
socket_listen($sock);
for(;;){
$sock=socket_accept($sock);
$write_buffer="HTTP/1.0 200 OK\r\nServer: my_server\r\nContent-Type: text/html; charset=utf-8\r\n\r\nhello!world";
$socket_write($conn,$write_buffer);
$socket_close($conn);
}
```
运行:
```
php server.php
```