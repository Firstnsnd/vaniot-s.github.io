---
title: linux下安装和卸载软件
date: 2018-01-16 15:32:23
tags: linux
categories: basic
---
linux(继承自Unix)的文件系统架构，系统会默认选择安装目录，通常情况下:

- 程序的文档->/usr/share/doc; /usr/local/share/doc
- 程序->/usr/share; /usr/local/share
- 程序的启动项->/usr/share/apps; /usr/local/share
- 程序的语言包->/usr/share/locale; /usr/local/share/locale
- 可执行文件->/usr/bin; /usr/local/bin
- 配置文件-> /etc
- lib文件->/usr/lib
例如:系统安装软件一般在/usr/share，可执行的文件在/usr/bin，配置文件可能安装到了/etc下等。

编程语言的分类：
1.编译型语言: 在程序执行前，会编译成机器语言，再次执行不会编译
2.解释型语言: 在程序执行时翻译为机器语言
>ps:脚本语言：是一种解释性语言，不需要编译，由相应的脚本引擎解释执行

## 一、编译、安装、运行程序
   1.编写c语言程序
    ```
       vi HelloWorld.c
     ```
   2.编写程序
    ![c语言输出hello world](https://raw.githubusercontent.com/Vaniot-s/picture/master/2018-01-16%2020-00-27.png)
 3.使用gcc编译为二进制文件
    ```
    gcc HelloWorld.c -o HelloWorld
    ```
  4.运行程序
     (1)使用路径
     ![输入路径运行程序](https://raw.githubusercontent.com/Vaniot-s/picture/master/2018-01-16%2020-33-27.png)
     (2)利用环境变量
  > 查看环境变量(环境变量由冒号分割开，使用命令在路径中去寻找该命令,使用which时若无该命令无输出)
       ![ubuntu环境变量](https://raw.githubusercontent.com/Vaniot-s/picture/master/2018-01-16%2021-10-54.png)
 
 - 将二进制文件复制到任意PATH变量包含的目录中
  ![ubuntu环境变量](https://raw.githubusercontent.com/Vaniot-s/picture/master/2018-01-16%2021-32-51.png)
  -  将文件所在的目录添加到PATH变量中(先要确保已存在的路径中无该二进制文件)
  ![ubuntu环境变量](https://raw.githubusercontent.com/Vaniot-s/picture/master/2018-01-16%2022-01-08.png)
  > 上述添加目录到PATH变量的方法在主机重启后会失去作用，并未将定义的环境变量保存到任何配置文件中
  ```
  echo "export PATH=$PATH:/home/vaniot" >>/etc/rc.local
  ```
  
  源码编译安装软件原理:
       源代码->源码编译生成可执行的文件->复制该文件到任一PATH变量包含的目录
## 二、源码包编译安装
### 1.典型源码编译安装软件
   (1).运行configure命令，结合必要的参数生成Makefile;
   (2).运行make命令生成各类模板和主程序
   (3).运行make install命令将必要的文章复制到安装目录中
   
#### 使用ubuntu自带apt-get安装加Node.js 
在终端中执行：
```
sudo add-apt-repository ppa:chris-lea/node.js //ubuntu自带的node版本太老，PPA安装最新版的Node.js，
sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
```
>更新node和npm

升级npm为最新版本
```
sudo npm install npm@latest -g
```
安装用于安装nodejs的模块n
```
sudo npm install -g n
```
然后通过n模块安装指定版本的nodejs，n模块更多介绍请参考[官方文档](https://www.npmjs.com/package/n)
```
//安装官方最新版本
sudo n latest
//安装官方稳定版本
sudo n stable
//安装官方最新LTS版本
sudo n lts
```