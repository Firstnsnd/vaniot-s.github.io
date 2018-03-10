---
title: WebSocket协议
mathjax: true
date: 2018-03-09 22:36:50
tags: websocket
categories: web
---
### WebSocket的特点：
- 保留HTTP特性，url,http安全性，更简单的基于数据模型的消息和内置的文本支持
- 异步：可用做高级协议的传输层，是消息协议，聊天，服务器通知，管道和多路复用协议，自定义协议，紧凑二进制协议及用于与互联网服务器操作的其他标准协议的良好基础。
TCP,HTTP和WebSocket对比

特性|TCP|HTTP|WebSocket
--|--|--|--
寻址|IP地址和端口|URL|URL
并发传输|全双工|半双工|全双工
内容|字节流|MIME消息|文本和二进制消息
消息定界|否|是|是
连接定向|是|否|是

WebSocket传输一序列单独的消息，内置消息边界，多字节的消息做为整体，按照顺序到达。
### WebSocket初始握手
每个WebSocket连接都始于一个HTTP请求，较其他请求包含了一个特殊的首标---Upgrade(表示客户端把连接升级到不同的协议)

客户端发送的HTTP请求(WebSocket初始握手):
```
GET /echo HTTP/1.1
Host:echo.websoket.org
Origin:http://www.websocket.org
Sec-WebSocket-Key:7+C600xYybO2zmJ69RQSW==
Sec-WebSocket-Version:13
Upgrade:websocket
```
服务器发出的HTTP响应：
```
101 Switching Protocols
Connection: Upgrade
Date: Wed,20 Jun 2012 03:39:45 GMT
Sec-WebSocket-Accept:fYoqiH14DgI+5y1EMwM2sOLzOio= //响应键值
server:Kaazing Gateway
Upgrade:WebSocket
```
1.计算响应键值
  WebSocket服务器必须响应一个计算出的键值，表明服务器理解WebSocket协议。响应函数从客户端发送的Sec-WebSocket-Key首标中取得键值，并在Sec-WebSocket-Accept首标中返回根据客户端预期计算的键值。