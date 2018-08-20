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
   - `.env`文件实现大部分配置。
   -  (Laravel)发布的配置文件或(Lumen)`bootstrap/app.php`中配置
   - AppserviceProvider的boot方法

### env中的配置
#### 标准树(API_STANDRDS_TRUE)
标准树有三个不同的值
- x:未注册的，树用于本地或私有环境
- prs:个人树，用于非商业销售的项目
- vnd:供应商树，主要用于公开的及商业销售的项目

```
API_STANDARD_TREE=vnd
```
#### 子类型(API_SUBTYPE)
子类型通常是应用或项目的简称，
```
API_SUBTYPE=BG
``` 
#### 前缀和子域名
API一般以子域名的形式提供服务，前缀和子域名是必需的，同时只能有一个。
配置前缀
```
API_PREFIX=api
```
配置子域名
```
API_DOMAIN=api.myapp.com
```
####  默认的版本
在api.php中未设置API的版本时，默认为`.env`该配置
```
API_VERSION=V
```
#### API名字
API的名字在使用API Blueprint命令生成文档时会用到。
```
API_NAME=My API
```
#### 带条件的请求
缓存API请求的时候会使用客户端缓存功能，默认开启带条件的请求
```
API_CONDITIONAL_REQUEST=false
```
#### Strict模式
Strict模式要求客户端发送Accept头而不是默认在配置文件中指定的版本。即不能通过Web浏览器浏览API。当Strict开启且是用了无效的Accept头 ，API会抛出Symfony\Component\HttpKenel\Exception\BadRequestHttpException异常。
```
API_STRICT=false
```
#### 调试模式
 开启调试模式，生成的错误信息将会被扩展包放到`debug`键中，并与堆栈跟踪信息一起显示
 ```
 API_DEBUG=true
```
