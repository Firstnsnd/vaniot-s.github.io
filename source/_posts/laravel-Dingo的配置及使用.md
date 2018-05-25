---
title: Laravel-Dingo的配置及使用
date: 2018-05-08 12:36:07
tags: [Laravel,Dingo]
catrgories: web
---
## 一.安装及基础配置
   dingo环境要求：
    - laravel(或Lumen)5.1+
    - PHP 5.5.9 +
    
#### 下载
 安装使用Composer命令安装扩展包:
 ```
  composer require dingo/api:1.0.x@dev
  ```
  <!--more-->
##### Laravel基础配置
 在laravel框架下，注册服务提供者。config/app.php中的providers数组中添加如下代码
 ```
 Dingo\Api\Provider\LaravelServiceProvide::class
 ```
需要生成自定义的dingo配置文件config/api.php(发布的配置文件),在Termianl中执行
```
php artisan vendor:publis --provider="Dingo\Api\Provider\LaravelServiceProvider::class"
```
##### Lumen基础配置
打开bootstrap/app.php注册服务提供者:
```
$app->register(Dingo\Api\Provider\LumenServiceProvider::class);
```
#####  门面
 添加下面两个门面中的任意一个扩展包：
  - Dingo\Api\Facade\API
  - Dingo\Api\Facade\Route

## 二、使用的基础配置
 安装的过程中功能都已预先配置完成，更多的配置可在：
   - `.env`文件实现大部分配置，其中配置形式为`API_XXXX=XX`。
   -  (Laravel)发布的配置文件或(Lumen)`bootstrap/app.php`中配置
   - AppserviceProvider的boot方法

### 标准树(API_STANDRDS_TRUE)
标准树有三个不同的值
- x:未注册的，树用于本地或私有环境
- prs:个人树，用于非商业销售的项目
- vnd:供应商树，主要用于公开的及商业销售的项目

```
API_STANDARD_TREE=vnd
```
### 子类型(API_SUBTYPE)
子类型通常是应用或项目的简称，
```
API_SUBTYPE=BG
``` 
### 前缀和子域名
API一般以子域名的形式提供服务，前缀和子域名是必需的，同时只能有一个。
配置前缀
```
API_PREFIX=api
```
配置子域名
```
API_DOMAIN=api.myapp.com
```
###  默认的版本
在api.php中未设置API的版本时，默认为`.env`该配置
```
API_VERSION=V
```
### API名字
API的名字在使用API Blueprint命令生成文档时会用到。
```
API_NAME=My API
```
### 带条件的请求
缓存API请求的时候会使用客户端缓存功能，默认开启带条件的请求
```
API_CONDITIONAL_REQUEST=false
```
### Strict模式
Strict模式要求客户端发送Accept头而不是默认在配置文件中指定的版本。即不能通过Web浏览器浏览API。当Strict开启且是用了无效的Accept头 ，API会跑出Symfony\Component\HttpKenel\Exception\BadRequestHttpException异常。
```
API_STRICT=false
```
#### 调试模式
 开启调试模式，生成的错误信息将会被扩展包放到`debug`键中，并与堆栈跟踪信息一起显示
 ```
 API_DEBUG=true
 ```
 
### 认证提供者

默认只开启了basic认证，如果要对认证进行更复杂的配置，需要在服务提供者（Laravel）或启动文件（Lumen）设置：
```
$app['Dingo\Api\Auth\Auth']->extend('oauth', function ($app) {
   return new Dingo\Api\Auth\Provider\JWT($app['Tymon\JWTAuth\JWTAuth']);
});
```
### Throttling/频率限制

默认频率限制是被禁用的，你可以注册自定义的带有频率限制的throttle或者使用已经存在的认证及取消认证throttle。

要进行更加复杂的配置同样需要在服务提供者或启动文件中操作：
```
$app['Dingo\Api\Http\RateLimit\Handler']->extend(function ($app) {
    return new Dingo\Api\Http\RateLimit\Throttle\Authenticated;
});
```
### 响应转化器

Fractal是默认的响应转化器。你可以在.env中配置，但如果要实现更加复杂的配置还是需要在服务提供者或启动文件操作：
```
$app['Dingo\Api\Transformer\Factory']->setAdapter(function ($app) {
    $fractal = new League\Fractal\Manager;

    $fractal->setSerializer(new League\Fractal\Serializer\JsonApiSerializer);

    return new Dingo\Api\Transformer\Adapter\Fractal($fractal);
});
```
### 响应格式

默认的响应格式是JSON，可以在配置文件.env中配置默认的响应格式：
```
API_DEFAULT_FORMAT=json
```
更加高级的格式配置需要发布配置文件`config/api.php`或者在服务提供者及启动文件中操作：
```
Dingo\Api\Http\Response::addFormatter('json', new Dingo\Api\Http\Response\Format\Jsonp);
```
### 错误格式

如果扩展包遇到错误将会尝试生成错误响应而不是将异常抛给用户，错误格式可以进行自定义配置。Laravel中该配置必须在`config/api.php`目录下对应的配置文件或启动文件（Lumen）中实现：
```
$app['Dingo\Api\Exception\Handler']->setErrorFormat([
    'error' => [
        'message' => ':message',
        'errors' => ':errors',
        'code' => ':code',
        'status_code' => ':status_code',
        'debug' => ':debug'
    ]
]);
```
## 二、使用API路由(API Endpoint)
