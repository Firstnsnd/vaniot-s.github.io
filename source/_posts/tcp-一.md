---
title: TCP(一)
date: 2018-03-20 22:40:44
tags: [socket,php]
categories: web
---
###  TCP协议基本工作流程
#### TCP协议的特点：
   - 面向连接
   - 字节流协议
   - 全双工
   - 可靠的差错控制和流量控制 
   
#### TCP 三次握手：
 - 客户端主动调用connect发送SYN分节
 - 服务器必须回复一个ACK分节来确认客户端SYN分节，并发送一个SYN分解到客户端
 - 客户端对服务端发送的SYN分节进行ACK确认
 ![tcp三次握手](https://raw.githubusercontent.com/Vaniot-s/picture/master/tcp/handle.jpg)
<!--more-->

#### TCP四次挥手：
 - 首先申请拆除的一端调用 close 发送一个 FIN 分节
- 另一端接收到 FIN 分节时，发送一个 ACK 分节进行确认
- 同理，另一端要申请拆除连接时，也要发送一个 FIN 分节
- 接收端发送 ACK 分节进行确认
![tcp四次挥手](https://raw.githubusercontent.com/Vaniot-s/picture/master/tcp/waved.jpg)

#### TCP状态转换

- SYN_SENT 主动打开，SYN分节已发送
- SYN_RCVD被动打开，SYN分节已经接收
- ESTABLISHED已经建立连接

- FIN_WAIT_1发起主动关闭，FIN分节已发送
- CLOSE_WAIT被动关闭，FIN分节已接收，ACK分节已发送
- FIN_WAIT_2 成功实现半关闭，ACK分节已接收
- LAST_ACK最终的ACK，FIN分节已发送
- TIME_WAIT FIN分节已接收，ACK分节已发送
- CLOSED ACK分节已接受，成功拆连接
![TCP状态转换](https://raw.githubusercontent.com/Vaniot-s/picture/master/tcp/translate.png)