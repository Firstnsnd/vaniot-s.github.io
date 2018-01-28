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
## 一、软件包管理器
dpkg(跟red hat的rpm非常相似)和apt-get进行安装软件，dpkg与apt-get的区别：

-  dpkg绕过apt包管理数据库对软件包进行操作，用dpkg安装过的软件包用apt可以再安装一遍，系统不知道之前是否安装过，会覆盖之前dpkg的安装。
- dpkg是用来安装.deb文件,不会解决模块的依赖关系,不会关心ubuntu的软件仓库内的软件,可以用于安装本地的deb文件。
- apt会解决和安装模块的依赖问题,并会咨询软件仓库, 不会安装本地的deb文件, apt是建立在dpkg之上的软件管理工具。
### 1.dpkg
（1）安装软件

          命令行：dpkg -i <.deb file name>
          示例：dpkg -i remarkable_1.87_all.deb
（2）安装一个目录下面所有的软件包

          命令行：dpkg -R
          示例：dpkg -R /usr/local/src
（3）释放软件包，但是不进行配置

        命令行：dpkg –-unpack package_file 如果和-R一起使用，参数可以是一个目录
        示例：dpkg –-unpack remarkable_1.87_all.deb
（4）重新配置和释放软件包

        命令行：dpkg –configure package_file
        如果和-a一起使用，将配置所有没有配置的软件包
        示例：dpkg –configure remarkable_1.87_all.deb
（5）删除软件包（保留其配置信息）

        命令行：dpkg -r
        示例：dpkg -r remarkable_1.87_all
（6）替代软件包的信息

        命令行：dpkg –update-avail <Packages-file>
（7）合并软件包信息 

        dpkg –merge-avail <Packages-file>
（8）从软件包里面读取软件的信息

        命令行：dpkg -A package_file
（9）删除一个包（包括配置信息）

        命令行：dpkg -P
（10）丢失所有的Uninstall的软件包信息

        命令行：dpkg –forget-old-unavail
（11）删除软件包的Avaliable信息

        命令行：dpkg –clear-avail
（12）查找只有部分安装的软件包信息

        命令行：dpkg -C
（13）比较同一个包的不同版本之间的差别

        命令行：dpkg –compare-versions ver1 op ver2
（14）显示帮助信息

        命令行：dpkg –help
（15）显示dpkg的Licence

        命令行：dpkg –licence (or) dpkg –license
（16）显示dpkg的版本号

        命令行：dpkg --version
（17）建立一个deb文件

        命令行：dpkg -b directory [filename]
（18）显示一个Deb文件的目录

        命令行：dpkg -c filename
（19）显示一个Deb的说明

        命令行：dpkg -I filename [control-file]
（20）搜索Deb包

        命令行：dpkg -l package-name-pattern
        示例：dpkg -l vim
（21）显示所有已经安装的Deb包，同时显示版本号以及简短说明

         命令行：dpkg -l
（22）报告指定包的状态信息

        命令行：dpkg -s package-name
        示例：dpkg -s ssh
（23）显示一个包安装到系统里面的文件目录信息(使用apt-get install命令安装)

        命令行：dpkg -L package-Name
        示例：dpkg -L apache2
（24）搜索指定包里面的文件（模糊查询）

        命令行：dpkg -S remarkable
（25）显示包的具体信息

        命令行：dpkg -p package-name
        示例：dpkg -p remarkable
   (26)重新配制一个已经安装的包裹，如果它使用的是 debconf
   
          dpkg-reconfigure --frontend=dialog debconf //重新配制 debconf，使用一个 dialog 前端
    (27)获取当前状态 
    
        dpkg --get-selections *wine*
        
### 2.apt-get
## 二、源码包编译安装
###  1、编译、安装、运行程序
编程语言的分类：
1.编译型语言: 在程序执行前，会编译成机器语言，再次执行不会编译
2.解释型语言: 在程序执行时翻译为机器语言
>ps:脚本语言：是一种解释性语言，不需要编译，由相应的脚本引擎解释执行

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

### 2.典型源码编译安装软件
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