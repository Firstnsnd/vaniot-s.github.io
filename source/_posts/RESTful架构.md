---
title: RESTful架构
date: 2018-10-31 01:00:43
tags: RESTFUL
categories: web
---
RESTful(Representational State Transfer):表征性状态传输,RESTful架构又被称作为“面向资源开发”,将任何可命名的概念视为资源，在开发的过程中以`名词为核心`，故设计时尽量使用名词，禁止使用动词，而是使用HHTP动词，对服务端资源进行操作，从而实现“表现层状态转化”。
<!--more-->
## RESTful的URI设计
- 使用`—`或`-`连接名词提升易读性，如:`http://www.example.com/restful-archiecture`
- 使用`/`表示资源的层级关系,如：`http://www.example.com/books/gone-with-wind`
- 使用`，`或者`;`表示同级资源的关系，如:`http://www.example.com/gone-with-wind;Jane-Eyre`
- 使用`?`过滤资源，`http://www.example.com/books?category=science`
## 统一资源接口(HTT动作)
RESTful架构遵循统一的接口原则，包含一组受限的预定义的操作，所有的资源通过相同的接口进行资源访问。

|动作|操作|
|--|--|
|GET(SELECT)|从服务器检索特定资源(或列表)|
|POST(CREATE)|在服务器上创建一个新的资源|
|PUT(UPDATE)|更新服务器上的整个资源|
|PATCH(UPDATE)|更新服务器上的部分资源|
|DELETE(DELETE)|从服务器上删除资源|

### 用法
#### GET
-  安全且幂等，获取表示，变更时获取表示（缓存）

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

#### POST
- 不安全且不幂等，使用服务端管理的（自动产生）的实例号创建资源，创建子资源，部分更新资源，如果没有被修改，则不过更新资源（乐观锁）

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

#### PUT

- 不安全但幂等用客户端管理的实例号创建一个资源，通过替换的方式更新资源，如果未被修改，则更新资源（乐观锁）
   
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

#### DELETE
-  不安全但幂等，用于删除资源
    
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

>幂等：指的是同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态也是一样的，幂等方法不应该具有副作用(是否有客户端请求产生）。
>ps 少见的HTTP动词

- HEAD - 检索有关资源的元数据，例如数据的哈希或上次更新时间。
- OPTIONS - 检索关于客户端被允许对资源做什么的信息。

>请求例子

```
GET /collection #获取所有的集合列表
POST /collection #新建一个集合
GET /collection/:id #获取指定集合的信息
PUT /collection/:id #更新指定集合的全部信息
PATCH /collection/:id #更新指定集合的部分信息
DELETE /collection/:id #删除指定的集合
```
## 客户端与服务端
  客户端获取资源的表述（资源的外在呈现)不是资源的本身，资源的的表述包含了数据的描述和数据的元数据，浏览器与服务器间通过HTTP协议协商，浏览器通过`Accept`头请求一种特定的格式表述，服务器通过`Content-type`返回给浏览器客户端资源的表述形式。
  ![资源表述](https://raw.githubusercontent.com/vaniot-s/picture/master/RESTful/resourcedecrption.jpg)
## 状态转移
  在REST设计原则中客户端与服务端的交互无状态，客户端维护应用的状态，服务端维护资源的状态，但服务端不保存客户端状态。只有在每一次的请求中包含处理请求的信息，"会话"被客户端用作为应用状态进行跟踪，在服务端的超媒体的指引下发生变迁。服务端通过超媒体告诉客户端当前状态有哪些后续状态可以进入。 
  
> 后记
  - 违反无状态通信原则的设计,如:利用Cookie跟踪某个服务端会话状态.
  - 超媒体:把一个个把资源链接起来.