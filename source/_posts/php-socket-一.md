---
title: php socket(一)
date: 2018-03-27 13:12:02
tags: [socket,php]
---
## Socket函数
### 一、socket
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
$sock=socket_create(AF_INET,SOCK_STREAM,SOL_TCP);//创建socket资源
socket_bind($sock,"127.0.0.1",80);//绑定端口
socket_listen($sock);//监听端口
for(;;){
$sock=socket_accept($sock);//接收向端口发出的请求
$write_buffer="HTTP/1.0 200 OK\r\nServer: my_server\r\nContent-Type: text/html; charset=utf-8\r\n\r\nhello!world";
$socket_write($conn,$write_buffer);//写入数据到socket中
$socket_close($conn);
}
```
运行:
```
php server.php
```
## 二、socket_stream
### stream_socket_server
一次性创建、绑定端口、监听端口
函数: `resource stream_socket_server ( string $local_socket [, int &$errno [, string &$errstr [, int $flags = STREAM_SERVER_BIND | STREAM_SERVER_LISTEN [, resource $context ]]]] )`
- local_socket: 协议名://地址:端口号
- errno: 错误码
- errstr: 错误信息
- flags: 只使用该函数的部分功能
- context: 使用stream_context_create函数创建的资源流上下文
### stream_socket_accept
函数原型: `resource stream_socket_accept ( resource $server_socket [, float $server_socket[,float timeout = ini_get("default_socket_timeout") [, string &$peername ]] )`

- server_socket: 使用stream_socket_server创建的stream资源
- timeout: 超时时间
- peername: 设置客户端主机名称

```php
// server.php
<?php 
$sock = stream_socket_server("tcp://127.0.0.1:80", $errno, $errstr);
for ( ; ; ) {
    $conn = stream_socket_accept($sock);
    $write_buffer = "HTTP/1.0 200 OK\r\nServer: my_server\r\nContent-Type: text/html; charset=utf-8\r\n\r\nhello!world";
    fwrite($conn, $write_buffer);
    fclose($conn);
}
```
