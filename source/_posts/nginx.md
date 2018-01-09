---
title: nginx(一)
date: 2018-01-08 21:48:05
tags: [nginx,服务器]
categories: web
---
### 一.常见的服务器
|nginx|iis|apache|tomcat|Lighthttpd
--|--|--|--|--
用途|http server、反向代理(reverse proxy)、IMAP/POP3代理,nginx官方网站 http://www.nginx.org |Microsoft公司的WEB服务器产品，支持Gopher Server、FTP Server、HTTP Server、NNTP Server、SMTP Server 官方网站 http://www.iis.net |拓展功能最丰富，工具特性最全，官方网站：http://httpd.apache.org |apache的拓展部分，为运行JSP和Servlet提供服务|开源轻量级web服务器软件，支持[FastCGI](https://zh.wikipedia.org/wiki/FastCGI),Output Compress,URL重写
### 二.Nginx的功能特性
#### 1.基本的HTTP服务 
   - 处理静态文件，处理索引文件及支持自动索引
   - 打开并自行管理文件描述符缓存
   - [反向代理服务](https://zh.wikipedia.org/wiki/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86)，使用缓存加速反向代理，同时完成负载均衡及容错
   - 远程FastCGI服务的缓存机制，加速访问，同时完成负载均衡及容错
   - 使用Nginx的模块化特性提供过滤器功能。Nginx基本过滤器包括gzip压缩，ranges支持、chunked响应，XSLT、SSI以及图像放缩(ps:对包含多个SSI的页面，经由FastCGI或反向代理，SSI过滤器可以并行处理)
   - 支持HTTP下的安全套接层安全协议SSL
#### 2.高级HTTP服务
   - 基于名字和IP的虚拟主机设置
   - HTTP/1.0中的[KEEP-Alive(维基百科)](https://zh.wikipedia.org/wiki/HTTP%E6%8C%81%E4%B9%85%E8%BF%9E%E6%8E%A5)/[MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Keep-Alive)模式和[管线(PipeLined)](https://zh.wikipedia.org/wiki/HTTP%E7%AE%A1%E7%B7%9A%E5%8C%96)模型连接
   - 重新加载配置及在线升级时，无须中断正在处理的请求
   - 自定义访问日志格式，带缓存的日志写操作及快速日志轮转
   - 可3XX~5XX错误代码重定向功能
   - 支持HTTP DAV模块，从而为HTTP WebDAV提供PUT、DELETE、MKCOL、COPY、MOVE方法
   - 支持FLV流和MP4流
   - 支持网络监控：客户端IP、HTTP基本认证机制的访问控制，速度限制，来自同一地址的同时连接数或者请求数限制
   - 支持嵌入Perl语言
#### 3.邮件代理服务器
   - 可使用外部HTTP认证服务器重定向用户到IMAP/POP3后端，并支持IMAP认证方式(LOGIN、AUTHLOGIN/PLAIN/CRAM-MD5)和POP3认证(USER/PASS、APOP、AUTHLOGIN/PLAIN/CRAM-MD5)
   - 可使用外部HTTP认证服务器重定向用户到STMP后端，并支持STMP认证方式(AUTHLOGIN/PLAIN/CRAM-MD5)
   - 支持邮件代理服务下的安全层协议SSL
   - 支持纯文本通信协议的拓展协议STARTTLS
PS:三种邮件协议的区别：[这里](/三种邮件协议-STMP、POP3、IMAP/)
### 三、nginx的安装与配置
