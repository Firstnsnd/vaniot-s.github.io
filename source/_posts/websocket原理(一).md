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
构造函数：在第一次WebSocket连接握手时，客户发送带有协议名称的Sec-WebSocket-Protocol首标,服务器选择0个或者1个协议。
- 参数一：URL(必须)，指向连接目标的URL
    - ws客户端与与服务端非加密流量
    - wss客户端与服务端加密流量
-  参数二:protocol(可选),应用程序支持的协议，服务器必须在响应中包含一个或一组协议名称，用于协议协商，建立在WebSocket之上使用，可使用:
   - 注册协议：根据RFC6455,向IANA中正式注册的标准协议，如：SOAP
   - 开放协议:如STOMP,XMPP
   - 自定义协议
### 2.  WebSocket事件
应用程序监听事件，采用回调函数或DOM方法addEventListener()为WebSocket对象添加事件监听器。
  - open：open事件触发协议握手完成，建立一个连接，可以发送和接受数据。回调函数为onopen
  ```
  ws.onopen=function(e){
      console.log("Connection open...");
  }
  ```
  - message: message事件在接收到消息(由WebSocket Frame组成)时触发，对应的回调函数为onmessage
  文本消息：
  ```
  ws.onmessage=function(e){
   if(typeof e.data==="string"){
     console.log("String message received",e,e.data);
    }else{
      console.log("Other message received",e,e.data);
    }
  }
  ```
  二进制数据：
  - Blob
  ```
  ws.binaryType="bllob";
  ws.onmessage=function(e){
  if(e.data instanceof Blob){
  console.log("Blob message received",e.data);
  var blob=new Blob(e.data);
  }
  }
  ```
   - ArrayBuffer
  ```
  ws.binaryType="arraybuffer";
  ws.onmessage=function(e){
  if(e.data instanceof ArrayBuffer){
  console.log("ArrayBuffer message received",e.data);
  var blob=new Unit8(e.data);
  }
  }
  ```
  - error: 响应意外故障时触发，导致WebSocket连接关闭，触发close事件。
  ```
  ws.onerror=function(e){
  console.log("WebSocket Error:",e);
  handErrors(e);
  }
  ```
  - close: WebSocket连接关闭时触发，对应于close事件的回调函数是onclose,
  ```
  ws.onclose=function(e){
  console.log("Connection closed",e);
  }
  ```
close的3个属性:

属性|说明
--|--
wasClean|布尔属性表示连接是否顺利关闭
code|服务器发送的关闭握手状态
error|服务器发送的关闭握手状态

### 3.WebSocket方法
1.send()
 WebSocket建立全双工双向连接后，可在打开连接时调用send()方法。可用属性readyState检查在套接字打开时发送数据。
 ```
 var ws=new WebSocket("ws:xx.com")
 ws.onopen=function(){
      myEventHandler(data)
 }
 function myEventHandler(data){
 if(ws.readyState===WebSocket.OPEN){
     ws.send(data);
 }else{
   return "ssss";
 }
 }
//发送二进制数据
var blob=new Blob("blob");
ws.send(blob);

var a=new Unit8Array([5,6,3]);
ws.send(a.buffer);
 ```
2.close()
   关闭或终止WebSocket连接
  不带参数：
  ```
  ws.close()
  ```
  带参数：
  - code 数字型状态码
  - reason 文本字符串
  ```
  ws.close(1000,"Closing normally");
  ```
### 4.WebSocket对象特性
1.readyState:只读特性报告其连接状态

特性常量|取值|状态
--|--|--
WebSocket.CONNECTING|0|连接进行中，但还未建立
WebSocket.OPEN|1|连接已经建立，消息可以在用户和服务器之间传递
WebSocket.CLOSING|2|连接正在进行关闭握手
WebSocket.ClOSED|3|连接已经关闭，不能打开
2.bufferedAmount
 busseredAmount检查往服务器的缓冲数据，已经进入队列，但未发送到服务器的字节数。
 ```
 var THRESHOLD=1024;
 var ws=new WebSocket("ws:dddd.com");
 ws.onopen=function(){
 setInterval(function(){
 if(ws.bufferedAmount<THRESHOLD){
 ws.send(getApplicationState());
 }
 },1000);
 }
 ```
 3.protocol
  - WebSocket构造函数的参数protocol,用于服务器让客户端理解并可在WebSocket上使用的协议。
  - 在WebSocket对象上，protocol特性包含在打开握手期间WebSocket服务器上选择的协议。
### 5.检查WebSocket支持

```
if(window.WebSocket){
console.log("This browser support WebSocket");
}else{
console.log("This browser not support WebSocket");
}
```
 对于不支持WebSocket使用填充插件(Ployfill,用于旧的浏览器，复制标准API的一个JavaScript程序库)，或者Kaazing等WebSocket供应商。
 