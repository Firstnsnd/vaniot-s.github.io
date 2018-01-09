---
title: 利用vultr搭建ss
date: 2017-12-30 16:26:08
tags: shadowsocks
categories: tool
---
### 一、服务端配置
Ubuntu上的安装步骤（全程root权限),购买主机进入终端,依次执行如下命令：
1.更新软件源
```
apt-get update
```
2.安装pip环境
```
apt-get install python-pip
```
3.安装shadowsocks
```
pip install shadowsocks
```
此时，如果出现了提示版本太低，则按照提示更新
```
pip install --upgrade pip
```
如果提示没有setuptools模块，则安装setuptools
```
pip install setuptools
```
4.如果刚才shadowsocks安装成功则跳过这一步，某则继续安装shadowsocks
```
pip install shadowsocks
```
5.编辑配置文件
```
 vim /etc/shadowsocks.json
##单用户：
{
    "server":"my_server_ip",
    "server_port":8388,//默认是8388，如果不行可以换成1024试试（这句注释删除）
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb"
}
## 多用户：
{
    "server":"my_server_ip",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password":{
        "447":"123456",
        "448":"123456"
     },
    "timeout":300,
    "method":"aes-256-cfb"
}
```


name|info
--|--
server|	服务器 IP (IPv4/IPv6)，注意这也将是服务端监听的 IP 地址
server_port	|服务器端口
local_port|	本地端端口
password|用来加密的密码
timeout	|超时时间（秒）
method	|加密方法，可选择 “bf-cfb”, “aes-256-cfb”, “des-cfb”, “rc4″, 等等。默认是一种不安全的加密，推荐用 “aes-256-cfb”
赋予文件权限
```
chmod 755 /etc/shadowsocks.json
```
安装以支持这些加密方式
```
apt-get install python-m2crypto
```
后台运行
```
ssserver -c /etc/shadowsocks.json -d start
```
查看运行的端口，看配置是否启用
```
netstat -lnp
```
停止命令
```
ssserver -c /etc/shadowsocks.json -d stop
```
设置开机自启动
```
vim /etc/rc.local
```
 加上如下命令：
```
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.
ssserver -c /etc/shadowsocks.json -d start
exit 0
```
### 二、客户端
到这儿[客户端下载](https://github.com/shadowsocks),安装好客户端后，填入配置的参数，启动客户端
> 若服务端安装的是python版，则windows下的ss客户端安装2.5.8

更多问题:来[逗比根据地 ](https://doub.io)