---
title: websocket基础(一)
date: 2018-01-01 16:45:37
tags: [websocket,web即时通信]
categories: web
---
WebSocket:全双工、双向、单套接字连接，异步、双向通信模式
## WebSocket API
 WebSocket API:是事件驱动，全双工连接建立后，不需要轮询服务器获得资源

### 1.WebSocket构造函数:WebSocket()
```
var ws=new WebSocket("ws:xxx.com"[,"protocol"])
```
- 参数一：URL必须
    - ws客户端与与服务端非加密流量
    - wss客户端与服务端加密流量
-  
2.