---
title: RESTful架构
date: 2018-10-31 01:00:43
tags: RESTFUL
categories: web
---
HTTP协议本身是一种面向资源的应用层协议，但对HTTP协议的使用实际上存在着两种不同的方式：一种是RESTful的，它把HTTP当成应用层协议，比较忠实地遵守了HTTP协议的各种规定；另一种是SOA的，它并没有完全把HTTP当成应用层协议，而是把HTTP协议作为了传输层协议，然后在HTTP之上建立了自己的应用层协议.
RESTful(Representational State Transfer 表述性状态转移):对基于HTTP的应用提供了一种设计原则,即:
![RESTFul](https://github.com/vaniot-s/picture/blob/master/RESTful/restful.png?raw=true)
## RESTFul定义的URL
RESTful架构又被称作为“面向资源开发”,将任何可命名的概念视为资源,资源可以是实体或抽象的概念，`URL`的命名以`名词为核心`.URL表达了被操作资源的位置,不使用动词,并要注意单复数区分.
<!--more-->
### RESTful的URL设计
- 使用`—`或`-`连接名词提升易读性，如:`http://www.example.com/restful-archiecture`
- 使用`/`表示资源的层级关系,如：
  ```text
  //访问文章列表
  http://www.example.com/posts
  //某篇文章
  http://example.com/posts/{post}
  //某篇文章下的评论列表
  http://www.example.com/{posts/post}/comments
  ```
- 使用`，`或者`;`表示同级资源的关系，如:`http://www.example.com/gone-with-wind;Jane-Eyre`
- 使用`?`过滤资源，使用'&'分割参数`http://www.example.com/books?category=science`
- 在URI中避免出现文件扩展名(例如.php,.aspx 和.jsp)。
## HTTP动词
RESTful架构遵循统一的接口原则，包含一组受限的预定义的操作，所有的资源通过相同的接口进行资源访问,使用HTTP方法,并遵守其语义.

|动作|操作|
|:--|:--|
|GET(SELECT)|从服务器检索特定资源(或列表)|
|POST(CREATE)|在服务器上创建一个新的资源|
|PUT(UPDATE)|更新服务器上的整个资源|
|PATCH(UPDATE)|更新服务器上的部分资源|
|DELETE(DELETE)|从服务器上删除资源|
|OPTION(OPTION)|跨域访问预请求|

### 用法
#### GET
- 安全且幂等
- 语义:法请求指定的资源

|状态码|状态含义|
|--|--|
|200（OK） | 表示已在响应中发出|
|204（无内容）|资源有空表示|
|301（Moved Permanently） | 资源的URI已被更新|
|303（See Other） | 其他（如，负载均衡）|
|304（not modified）| 资源未更改（缓存）|
|400 （bad request）| 指代坏请求（如，参数错误）|
|404 （not found）|资源不存在|
|406 （not acceptable）| 服务端不支持所需表示|
|500 （internal server error）| 通用错误响应|
|503 （Service Unavailable）| 服务端当前无法处理请求|
[GET更多...](https://tools.ietf.org/html/rfc7231#section-4.3.1)
#### POST
- 不安全且不幂等
- 语义:发送数据给服务器,创建资源. 

状态码|状态含义
--|--
200（OK）| 如果现有资源已被更改
201（created）| 如果新资源被创建
202（accepted）|已接受处理请求但尚未完成（异步处理）
301（Moved Permanently）| 资源的URI被更新
303（See Other）| 其他（如，负载均衡）
400（bad request）| 指代坏请求
404 （not found）|资源不存在
406 （not acceptable）|服务端不支持所需表示
409 （conflict）|通用冲突
412 （Precondition Failed）| 前置条件失败（如执行条件更新时的冲突）
415 （unsupported media type）| 接受到的表示不受支持
500 （internal server error）| 通用错误响应
503 （Service Unavailable）| 服务当前无法处理请求

[POST更多...](https://tools.ietf.org/html/rfc7231#section-4.3.3)
#### PUT

- 不安全但幂等
- 语义:用于新增资源或者使用请求中的有效负载替换目标资源的表现形式。
   
状态码|状态含义
--|--
200 （OK）|如果已存在资源被更改
301（Moved Permanently）| 资源的URI已更改
303 （See Other）|其他（如，负载均衡）
400 （bad request）|指代坏请求
404 （not found）| 资源不存在
406 （not acceptable）| 服务端不支持所需表示
409 （conflict）| 通用冲突
412 （Precondition Failed）| 前置条件失败（如执行条件更新时的冲突）
415 （unsupported media type）| 接受到的表示不受支持
500 （internal server error）|通用错误响应
503 （Service Unavailable）| 服务当前无法处理请求
[Put更多...](https://tools.ietf.org/html/rfc7231#section-4.3.4)
#### DELETE
- 不安全但幂等
- 语义:请求方法用于删除指定的资源。
    
状态码|状态含义
--|--
200 （OK）| 资源已被删除
301 （Moved Permanently）| 资源的URI已更改
303 （See Other）| 其他，如负载均衡
400 （bad request）| 指代坏请求
404 （not found）| 资源不存在
409 （conflict）|通用冲突
500 （internal server error）| 通用错误响应
503 （Service Unavailable）|服务端当前无法处理请求

>[幂等](https://developer.mozilla.org/zh-CN/docs/Glossary/%E5%B9%82%E7%AD%89)：指的是同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态也是一样的，幂等方法不应该具有副作用(统计用途除外）。幂等是分布式系统的一种特性,不论是SOA还是RESTful的Web API设计都应该考虑幂等性。

> 使用RestFul操作资源的例子
```
GET /collection #获取所有的集合列表
POST /collection #新建一个集合
GET /collection/:id #获取指定集合的信息
PUT /collection/:id #更新指定集合的全部信息
PATCH /collection/:id #更新指定集合的部分信息
DELETE /collection/:id #删除指定的集合
```
## 资源的表述
客户端通过HTTP获取资源的表述,如文本的表述格式可为:html,xml,json等,资源的表述包括数据和描述数据的元数据，例如，HTTP头“Content-Type” 就是这样一个元数据属性。

### JSON API
RestFul规定URL和HTTP Method的使用,未定义body的数据格式,JSON是最主流的网络传输格式
#### MIME类型
客户端请求头中`Content-Type`应该为`application/vnd.api+json`，并且在`Accept`中也必须包含`application/vnd.api+json`。如果指定错误服务器应该返回415或406状态码。


## 客户端与服务端
  客户端获取资源的表述（资源的外在呈现)不是资源的本身，资源的的表述包含了数据的描述和数据的元数据，浏览器与服务器间通过HTTP协议协商，浏览器通过`Accept`头请求一种特定的格式表述，服务器通过`Content-type`返回给浏览器客户端资源的表述形式。
  ![资源表述](https://raw.githubusercontent.com/vaniot-s/picture/master/RESTful/resourcedecrption.jpg)
## 状态转移
  在REST设计原则中客户端与服务端的交互是无状态(HTTP协议的特性)，客户端维护应用的状态，服务端维护资源的状态，但服务端不保存客户端状态。只有在每一次的请求中包含处理请求的信息，"会话"被客户端用作为应用状态进行跟踪，在服务端的超媒体的指引下发生变迁。服务端通过超媒体告诉客户端当前状态有哪些后续状态可以进入。 
  
> ps
  - 违反无状态通信原则的设计,如:利用Cookie跟踪某个服务端会话状态.
  - 超媒体:把一个个把资源链接起来.
##  认证机制
  认证机制的引入用于解决，RESTful的服务无状态，认证机制用于确定访问资源的用户(权限机制用于确定用户是否可以使用，修改，删除或创建资源，常与业务逻辑绑定)，

### 
>参考资料
 1.[json](http://jsonapi.org/format/)
 2.[细说API – 重新认识RESTful](https://insights.thoughtworks.cn/api-restful/)