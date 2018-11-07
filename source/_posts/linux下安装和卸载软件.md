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
<!--more-->
## 一、软件包管理器
dpkg(跟red hat的rpm非常相似)和apt-get进行安装软件，dpkg与apt-get的区别：

-  dpkg绕过apt包管理数据库对软件包进行操作，用dpkg安装过的软件包用apt可以再安装一遍，系统不知道之前是否安装过，会覆盖之前dpkg的安装。
- dpkg是用来安装.deb文件,不会解决模块的依赖关系,不会关心ubuntu的软件仓库内的软件,可以用于安装本地的deb文件。
- apt会解决和安装模块的依赖问题,并会咨询软件仓库, 不会安装本地的deb文件, apt是建立在dpkg之上的软件管理工具。
### 1.dpkg
```shell
dpkg -i remarkable_1.87_all.deb //安装软件
dpkg -R /usr/local/src  //安装一个目录下面所有的软件包
dpkg –-unpack remarkable_1.87_all.deb //释放软件包，但是不进行配置,如果和-R一起使用，参数可以是一个目录
dpkg –configure remarkable_1.87_all.deb //重新配置和释放软件包. 如果和-a一起使用，将配置所有没有配置的软件包
dpkg -r remarkable_1.87_all ///删除软件包（保留其配置信息）
dpkg –update-avail <Packages-file> //替代软件包的信息
dpkg –merge-avail <Packages-file> //合并软件包信息 
dpkg -A package_file //从软件包里面读取软件的信息
dpkg -P //删除一个包（包括配置信息）
dpkg –forget-old-unavail //丢失所有的Uninstall的软件包信息
dpkg –clear-avail //删除软件包的Avaliable信息
dpkg -C  //查找只有部分安装的软件包信息
dpkg –compare-versions ver1 op ver2  //比较同一个包的不同版本之间的差别
dpkg –help  //显示帮助信息
dpkg –licence (or) dpkg –license //显示dpkg的Licence
dpkg --version  //显示dpkg的版本号
dpkg -b directory [filename]  //建立一个deb文件
dpkg -c filename //显示一个Deb文件的目录
dpkg -I filename [control-file] //显示一个Deb的说明
dpkg -l vim  //搜索Deb包
dpkg -l //显示所有已经安装的Deb包，同时显示版本号以及简短说明
dpkg -s ssh  //报告指定包的状态信息
dpkg -L apache2 //显示一个包安装到系统里面的文件目录信息(使用apt-get install命令安装)
dpkg -S remarkable //搜索指定包里面的文件（模糊查询）
dpkg -p remarkable //显示包的具体信息
dpkg-reconfigure --frontend=dialog debconf //重新配制 debconf，使用一个 dialog 前端 重新配制一个已经安装的包裹，如果它使用的是 debconf
dpkg --get-selections *wine*  //获取当前状态 
```
### 2.apt-get
```
apt-get install packagename  //安装一个新软件包（参见下文的aptitude）
apt-get remove packagename  //卸载一个已安装的软件包（保留配置文档）
apt-get remove --purge packagename  //卸载一个已安装的软件包（删除配置文档）
apt-get autoremove packagename  //删除包及其依赖的软件包
apt-get autoremove --purge packagname  //删除包及其依赖的软件包+配置文件，比上面的要删除的彻底一点
dpkg --force-all --purge packagename  //有些软件很难卸载，而且还阻止别的软件的应用,用这个强制卸载，但是有点冒险。
apt-get autoclean //apt会把已装或已卸的软件都备份在硬盘上，所以假如需要空间的话，这个命令来删除您已卸载掉的软件的备份。
apt-get clean  //这个命令会把安装的软件的备份也删除，不会影响软件的使用。
apt-get upgrade  //更新软件包，apt-get upgrade不仅可以从相同版本号的发布版中更新软件包，也可以从新版本号的发布版中更新软件包，尽管实现后一种更新的推荐命令为apt-get dist-upgrade。在运行apt-get upgrade命令时加上-u选项很有用（即：apt-get -u upgrade)。这个选项让APT显示完整的可更新软件包列表。不加这个选项，你就只能盲目地更新。APT会下载每个软件包的最新更新版本，然后以合理的次序安装它们。在运行该命令前应先运行 apt-get update更新数据库，更新任何已安装的软件包。
apt-get dist-upgrade  //将系统升级到新版本。
apt-cache search string  //在软件包列表中搜索字符串。
aptitude  //周详查看已安装或可用的软件包。和apt-get类似，aptitude能够通过命令行方式调用，但仅限于某些命令——最常见的有安装和卸载命令。
由于aptitude比apt-get了解更多信息，能够说他更适合用来进行安装和卸载。
apt-cache showpkg pkgs  //显示软件包信息。
apt-cache dumpavail pkgs //打印可用软件包列表。
apt-cache show pkgs  //显示软件包记录，类似于dpkg –print-avail。
apt-cache pkgnames  //打印软件包列表中任何软件包的名称。
apt-file search filename  //查找包含特定文档的软件包（不一定是已安装的），这些文档的文档名中含有指定的字符串。apt-file是个单独的软件包。必须先使用apt-get install来安装他，然后运行apt-file update。如apt-file search filename输出的内容太多，您能够尝试使用apt-file search filename | grep -w filename（只显示指定字符串作为完整的单词出现在其中的那些文档名）或类似方法.
apt-cache search sorftname //查找包含特定文档的软件包
apt-cache madusion sorftname //查找包含特定文档的软件包
apt-cache policy sorftname //查找包含特定文档的软件包
```
#### **example**
>ubuntu使用apt-get搭配java环境

        apt-get install default-jdk
        apt-get install default-jre
        
> 使用ubuntu自带apt-get安装加Node.js 

在终端中执行：
```
# 当add-apt-repository 命令不能找到时先执行，apt-get install software-properties-common
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
## 二、源码包编译安装

编程语言的分类：
1.编译型语言: 在程序执行前，会编译成机器语言，再次执行不会编译
2.解释型语言: 在程序执行时翻译为机器语言
>ps:脚本语言：是一种解释性语言，不需要编译，由相应的脚本引擎解释执行

- 已经编译的二进制包 ——统称 binary，后缀可以是 .bin 或者.sh或者没有
- 不需要编译即可运行的比如Python 源代码——即 source code，使用python *.py 调用 
###  1、编译、安装、运行程序
   1.编写c语言程序
    
        vi HelloWorld.c
   2.编写程序
        ![c语言输出hello world](https://raw.githubusercontent.com/Vaniot-s/picture/master/2018-01-16%2020-00-27.png)
 3.使用gcc编译为二进制文件
 
        gcc HelloWorld.c -o HelloWorld
  4.运行程序(已经编译的二进制包 ——统称 binary，后缀可以是 .bin 或者.sh或者没有 )
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
#### 安装webdtorm
安装过程：
解压

        tar zxvf WebStorm-172.3544.10.tar.gz
移动

        sudo mv WebStorm-172.3544.10/ /opt/WebStorm-172.3544.10/
创建链接

        sudo ln -s /opt/WebStorm-172.3544.10/ /opt/WebStorm
启动

        /opt/WebStorm/bin/webstorm.sh
添加Dash图标
     
        启动webstorm->tools->Creat Desktop Entry
>为ubuntu下的wechat创建Dash创建图标[下载地址](https://github.com/geeeeeeeeek/electronic-wechat/releases/download/V2.0/linux-x64.tar.gz)

1.在 /usr/share/applications 目录下增加 wechat.desktop 

        sudo vim /usr/share/applications/wechat.desktop  
2.添加内容

         [Desktop Entry]  
         Name=wechat  
        Type=Application  
        Terminal=false  
        Comment=chat
        Exec=/opt/wechat/electronic-wechat
        Icon=/opt/wechat/resources/wechat.png
### 2.典型源码编译安装软件
   (1).运行configure命令，结合必要的参数生成Makefile;
   (2).运行make命令生成各类模板和主程序
   (3).运行make install命令将必要的文章复制到安装目录中
####  **从源码编译安装**
  1.建造基本编译环境：

        sudo apt-get install build-essential tcl  
   >tcl包执行make test,用于在执行make后,检验make的过程是否有错
   
  - 为编译的软件安装合适的依赖，若是apt关系的源里有这个软件，或找到依赖相同的软件，使用apt-get build-dep 补齐依赖关系 如nginx可以使用

        sudo apt-get build-dep nginx

## 三、查找软件安装的位置
1.find命令

        sudo find / -name node
 2.dpkg -L命令(件使用apt-get install命令安装)
 
        dpkg -L node
 3.whereis命令
 
         whereis node
4.locate命令 

         locate node
 5.which命令
 
        which node
 6.type命令
 
        type node
 7.软件相关名称
        dpkg --get-selections | grep ‘软件相关名称’
## 四、卸载方法
-  编译安装后卸载可以试用

         sudo make uninstall  //若程序维护者嵌入了相关命令
 >使用checkinstall处理编译的包安装则使用的deb/apt处理步骤

- 删除软件

         sudo apt-get remove –purge 软件名
        sudo apt-get autoremove  --purge 软件名称  // 删除系统不再使用的孤立软件
        sudo apt-get autoclean 软件名称                      // 清理旧版本的软件缓存
        dpkg -l |grep ^rc|awk ‘{print $2}’ |sudo xargs dpkg -P     ///清除残余的配置文件
 