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
  在REST设计原则中客户端与服务端的交互是无状态(HTTP协议的特性)，客户端维护应用的状态，服务端维护资源的状态，但服务端不保存客户端状态。只有在每一次的请求中包含处理请求的信息，"会话"被客户端用作为应用状态进行跟踪，在服务端的超媒体的指引下发生变迁。服务端通过超媒体告诉客户端当前状态有哪些后续状态可以进入。 
  
> ps
  - 违反无状态通信原则的设计,如:利用Cookie跟踪某个服务端会话状态.
  - 超媒体:把一个个把资源链接起来.
##  认证机制
  认证机制的引入用于解决，RESTful的服务无状态，认证机制用于确定访问资源的用户(权限机制用于确定用户是否可以使用，修改，删除或创建资源，常与业务逻辑绑定)，
### Cookie/Session（适用于WEB）
 在请求时在服务端创建一个Session对象(可以存于服务器内存，内存数据库，文件等),同时创建一系列的Cookie键值对保存于浏览器的，Cookie会在HTTP请求时一起发送，通过匹配Cookie与session对象匹配来实现状态管理。
### Basic Auth
 HTTP基本认证，客户端在请求时提供`用户名`和`口令`的形式登录验证，使用时将`用户名`和`口令`使用`:`链接，再base64加密后使用`Authorization`发送给服务端,在服务端直接使用字段验证
 ```javascript
 xhr = new XMLHttpRequest();
 var auth=user+':'+password;
 xhr.setRequestHeader('Authorization', auth);
 xhr.open('GET',url) 
 ```
### Token
   Token认证是在用户登录成功时会为其生成在一段时间有效唯一的token,将其保存客户端和服务端，用户在请求时带上token用于服务端验证。
### JWT 认证
JWT(JSON WEB Token)用于在身份提供者和服务提供者间传递被人整用户身份信息，便于从资源服务器获取资源，该Token可用于被认证和加密
#### Jwt认证流程
 1. 客户端调用登录接口（或者获取 token 接口），传入用户名密码。
 2. 服务端请求身份认证中心，确认用户名密码正确。
 3. 服务端创建 JWT，返回给客户端。
 4. 客户端拿到 JWT，进行存储（可以存储在缓存中，也可以存储在数据库中，如果是浏览器，可以存储在 Cookie 中）在后续请求中，在 HTTP 请求头中加上 JWT。
 5. 服务端校验 JWT，校验通过后，返回相关资源和数据。

#### JWT结构
JWT 是由三段信息构成的，第一段为头部（Header），第二段为载荷（Payload)，第三段为签名（Signature）。每一段内容都是一个 JSON 对象，将每一段 JSON 对象采用 BASE64 编码，将编码后的内容用. 链接一起就构成了 JWT 字符串。如下：
```
header.payload.signature
```
1. 头部（Header）

头部用于描述关于该 JWT 的最基本的信息，例如其类型以及签名所用的算法等。这也可以被表示成一个 JSON 对象。
```
{
"typ": "JWT",
"alg": "HS256"
}
```
在头部指明了签名算法是 HS256 算法。

2. 载荷（payload）

载荷就是存放有效信息的地方。有效信息包含三个部分：

 - 标准中注册的声明

 - 公共的声明

 - 私有的声明

标准中注册的声明（建议但不强制使用）：
```
iss：JWT 签发者

sub：JWT 所面向的用户

aud：接收 JWT 的一方

exp：JWT 的过期时间，这个过期时间必须要大于签发时间

nbf：定义在什么时间之前，该 JWT 都是不可用的

iat：JWT 的签发时间

jti：JWT 的唯一身份标识，主要用来作为一次性 token, 从而回避重放攻击。
```
公共的声明 ：

公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息. 在客户端可不应添加敏感信息。

私有的声明 ：

私有声明是提供者和消费者所共同定义的声明，不存放敏感信息，因为 base64 是对称解密的，意味着该部分信息可以归类为明文信息。

示例如下：
```
{ "iss": "Online JWT Builder",
 "iat": 1416797419,
 "exp": 1448333419,
 "aud": "www.primeton.com",
 "sub": "devops@primeton.com",
 "GivenName": "dragon",
 "Surname": "wang",
 "admin": true
}
```
3. 签名（signature)

创建签名需要使用 Base64 编码后的 header 和 payload 以及一个秘钥。将 base64 加密后的 header 和 base64 加密后的 payload 使用. 连接组成的字符串，通过 header 中声明的加密方式进行加盐 secret 组合加密，然后就构成了 jwt 的第三部分。

比如：`HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`

JWT 的优点：

1. 跨语言，JSON 的格式保证了跨语言的支撑
2. 基于 Token，无状态
3. 占用字节小，便于传输

关于 Token 注销：

Token 的注销，由于 Token 不存储在服务端，由客户端存储，当用户注销时，Token 的有效时间还没有到，还是有效的。所以如何在用户注销登录时让 Token 注销是一个要关注的点。一般有如下几种方式：

1. Token 存储在 Cookie 中，这样客户端注销时，自然可以清空掉
2. 注销时，将 Token 存放到分布式缓存中，每次校验 Token 时区检查下该 Token 是否已注销。不过这样也就失去了快速校验 Token 的优点。
3. 多采用短期令牌，比如令牌有效期是 20 分钟，这样可以一定程度上降低注销后 Token 可用性的风险。

### Oauth
用户在使用时不将用户信息传给服务提供者，客户端向授权服务器请求授权，用户成功授权后授权服务器向客户端发放令牌，客户端再使用令牌向资源服务器请求资源。
  Oatuh的登录流程：
  ![Oauth](https://raw.githubusercontent.com/vaniot-s/picture/master/Oauth/Oauth.png)
>详细 [理解OAuth 2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)