---
title: ubuntu+windows安装双系统及问题
date: 2018-01-25 07:26:16
tags: 系统
categories: basic
---
### windows下修复ubuntu引导
  昨天安装了双系统的笔记本中win10崩了，各种折腾依旧未能修复，平时没有备份的习惯，就只能重装了:tired_face:(备份是个好习惯)，重装之后，电脑默认是win10，因为Windows是不能引导Linux的，而每次Windows 10升级或恢复都会将Linux的启动引导覆盖掉，导致无法进入Linux。正常情况下如下：
![inital](https://raw.githubusercontent.com/Vaniot-s/picture/master/system/ubuntu%2Bwindows/inital.PNG)
重装之后则是直接进入win10，找回ubuntu步骤如下：
<!--more-->
1.制作ubuntu的启动u盘，按安装的设置为U盘启动,选择试用Try ubuntu without install
![inital](https://raw.githubusercontent.com/Vaniot-s/picture/master/system/ubuntu%2Bwindows/usb.PNG)
2.进入界面打开终端，执行以下命令
```
sudo su
sudo add-apt-repositoryppa:yannubuntu/boot-repair
apt-get update
apt-get install boot-repair
```
3.在dash中搜索boot-repair,选中recommand repair，静候几分钟，等其结束后关机，拔掉Ｕ盘，开机
[更多](http://www.linuxidc.com/Linux/2017-11/148278.htm)