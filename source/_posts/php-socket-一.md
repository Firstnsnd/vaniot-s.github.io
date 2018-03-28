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