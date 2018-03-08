---
title: websocket基础(一)
date: 2018-01-01 16:45:37
tags: [websocket,web即时通信]
categories: web
---
WebSocket:全双工、双向、单套接字连接，异步、双向通信模式
## WebSocket API
 WebSocket API:是事件驱动，遵循异步编程模式，应用程序监听WebSocket对象上的事件，处理输入数据和连接状态的改变。全双工连接建立后，不需要轮询服务器获得资源。

### 1.WebSocket构造函数:WebSocket()
```
var ws=new WebSocket("ws:xxx.com"[,"protocol"])
```
- 参数一：URL(必须)，指向连接目标的URL
    - ws客户端与与服务端非加密流量
    - wss客户端与服务端加密流量
-  参数二:protocol(可选),应用程序支持的协议，服务器必须在响应中包含一个或一组协议名称，用于协议协商，建立在WebSocket之上使用，可使用:
   - 注册协议：根据RFC6455,向IANA中正式注册的标准协议，如：SOAP
   - 开放协议:如STOMP,XMPP
   - 自定义协议
### 2.  WebSocket事件
应用程序监听事件，采用回调函数或addEventListener()。
- open
- message
- error
- close

