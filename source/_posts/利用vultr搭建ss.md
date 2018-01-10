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
1.windows,android,ios到这儿[客户端下载](https://github.com/shadowsocks),安装好客户端后，填入配置的参数，启动客户端
> 若服务端安装的是python版，则windows下的ss客户端安装2.5.8

2.ubuntu下安装ss客户端

步骤:
- 安装shadowsocks-qt5
经过测试，可能人品问题我用其他shadowssocks版本均失败，shadowsocks-qt5是可视化的一个版本，可用。
友情提示：在apt-get这类指令中，习惯性加sudo，保证你有足够权限执行命令
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5（找软件）
sudo apt-get update （更新你的软件库）
sudo apt-get install shadowsocks-qt5 (正式安装)
```
- 配置客户端
按供应商提供的账户信息配置好，如果信息正确，就会启动成功。杂项里头可以配置：设置成功自动启动。
![shadosocks-qt5](https://raw.githubusercontent.com/Vaniot-s/picture/master/shodwsocks-qt5.jpg)
- 配置浏览器
在windows和mac下都不用配置，直接使用系统默认就好。
![firefoxss](https://raw.githubusercontent.com/Vaniot-s/picture/master/firefoxss.png)
2.使用firefox，打开【连接设置】。(chrome浏览器有一个swichysharp插件)
3.一定选择要使用【手动配置代理】
4.默认有个http代理127.0.0.1，这个一定要清空。 
5.socks主机为socks5，地址和端口如图所填。
>说明：
1.在mac和windows下，可以使用pac模式，也就是在pac文件中设置哪些地址需要翻墙，不在列表内的地址就直连。
2.在ubuntu下默认是全局模式，也就是一律走代理翻，浏览器访问墙内的速度慢点。
3.当然我们也可以自己写pac，在浏览器设置中选择自动代理配置。不过貌似不能直接将windows下的搬过来。

更多问题:来[逗比根据地 ](https://doub.io)