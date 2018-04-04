---
title: php socket(一)
date: 2018-03-27 13:12:02
tags: [socket,php]
---
### Socket函数
#### socket_create
  函数 `resource socket_create(int $domain,int $typem,int $protocol)`
  
  +  domain:AF_INET、AF_INET6、AF_UNIX，*AF (address family)地址族*。
  + type: SOCK_STREAM、SOCK_DGRAM等，常用SOCK_STREAM，基于字节流的SOCKET类型，也是TCP协议使用的类型
+ protocol: SOL_TCP、SOL_UDP
<!--more-->
#### socket_bind
函数: `bool socket_bind(resource $socket,string $address[,int $port=0])`
- socket:使用`socket_create`创建的socket资源
- address：ip 地址
- port：监听的端口号，web服务器通常为80端口
#### socket_listen
函数 `bool socket_listen(resource $socket[,int $bcklog=0])`
- socket: 使用socket_create 创建的socket资源
- backlog: 等待处理连接队列的最大长度
#### socket_acccept
函数: `resource socket_accept(resource $socket)`
- socket:使用`socket_create`创建的socket资源