---
title: linux下安装软件
date: 2018-01-16 15:32:23
tags: linux
categories: basic
---
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
  
## 二、源码包编译安装
