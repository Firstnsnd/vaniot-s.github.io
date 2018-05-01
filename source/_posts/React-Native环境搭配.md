---
title: React-Native环境搭配
date: 2017-12-30 12:49:24
tags: React-Native
categories: web
---
#### 1.安装git for windows

在这里下载安装，安装过程中注意选择"Run Git from Windows Command Prompt"。(一路继续即可)
#### 2.安装nodejs
下载 建议设置npm镜像(git操作)以加速后面的过程（或使用科学上网工具）。
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
#### 3.安装C++环境
<!--more-->
推荐从itellyou下载并安装Visual Studio 2013或2015。也可选择Windows SDK、cygwin或mingw等其他C环境。编译node.js的C模块时需要用到。 如果使用VS2015，你需要在命令行中设置npm config set msvs_version 2015 --global

#### 4.安装Python
从官网下载并安装python 2.7.x（3.x版本不行）
### 5.安装JDK
下载JDK并安装。请注意选择x86还是x64版本。
环境变量配置
```
A、添加属性名称：JAVA_HOME
变量名：JAVA_HOME
变量值：G:\react\java\jdk
B、在PATH属性后添加
属性值：%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
C、添加属性名称CLASSPATH
属性值：.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
要加.表示当前路径，另外，%JAVA_HOME%就是引用前面指定的JAVA_HOME
```
#### 6. 关于环境变量是否安装成功的测
“开始”－>;“运行”，键入“cmd”； 键入命令命令，出现画面，说明环境变量配置成功：
```
A、java -version；
B、java；
C、javac；
```
#### 7.安装Android SDK

可以单独安装Android SDK，也可以通过Eclipse ADT或者Android Studio一并安装。推荐使用Android Studio，以下说明会默认以Android Studio的方式说明。请注意选择x86还是x64版本。 为了加速下载，推荐从AndroidDevTools下载。 然后进入SDKManager(可通过Android Studio菜单Tools-Android-SDK Manager)，确保以下项目已经安装并更新到最新：
```
Tools/Android SDK Tools (24.3.3)
Tools/Android SDK Platform-tools (22)
Tools/Android SDK Build-tools (23.0.1)（这个必须版本严格匹配23.0.1）
Android 6.0 (API 23)/SDK Platform (1) Extras/Android Support Library(23.0.1)
Extras/Local Maven repository for Support Libraries(之前叫做Android Support Repository)
```
添加系统环境变量ANDROID_HOME
```
G:\react\Android\sdk
系统path环境变量加入：
%ANDROID_HOME%;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\platforms;%ANDROID_HOME%\tools;
Android SDK配置完成，接下来验证配置是否成功。 点击运行——输入cmd——回车——输入adb——回车，如果出现一堆英文，即表示配置成功，在输入Android，启动Android SDK Manager。（运行android未成功过）
```
安装react-native命令行工具
```
npm install -g react-native-cli
//创建项目
```
进入你的工作目录，运行
```
react-native init MyProject
```
并耐心等待数（或数十）分钟,运行packager
```
react-native start
```
可以用浏览器访问[localhost](http://localhost:8081/index.android.bundle?platform=android) 看看是否可以看到打包后的脚本（看到很长的js代码就对了，即表示服务端无错）。第一次访问通常需要十几秒，并且在packager的命令行可以看到形如[====]的进度条。 
>如果你遇到了ERROR Watcher took too long to load的报错，请尝试修改node_modules/react-native/packager/react-packager/src/FileWatcher/index.js，将其中的MAX_WAIT_TIME 从25000改为更大的值（单位是毫秒） 运行模拟器

........

安卓运行
保持packager开启，另外打开一个命令行窗口，然后在工程目录下运行
```
react-native run-android 
```
  首次运行需要等待数分钟并从网上下载gradle依赖。（这个过程屏幕上可能出现很多小数点，表示下载进度。这个时间可能耗时很久，也可能会不停报错链接超时、连接中断等等——取决于你的网络状况和墙的不特定阻断。总之要顺利下载，请使用稳定有效的科学上网工具。） 运行完毕后可以在模拟器或真机上看到应用自动启动了。 
  
   如果apk安装运行出现报错，请检查上文中安装SDK的环节里所有依赖是否都已装全，platform-tools是否已经设到了PATH环境变量中，运行adb devices能否看到设备。 至此，应该能看到APP红屏报错，这是正常的，我们还需要让app能够正确访问pc端的packager服务。 摇晃设备或按Menu键（Bluestacks模拟器按键盘上的菜单键，通常在右Ctrl的左边 或者左Windows键旁边），可以打开调试菜单，点击Dev Settings，选Debug server host for device，输入你的正在运行packager的那台电脑的局域网IP加:8081（同时要保证手机和电脑在同一网段，且没有防火墙阻拦），再按back键返回，再按Menu键，在调试菜单中选择Reload JS，就应该可以看到运行的结果了。 如果真实设备白屏但没有弹出任何报错，可以在安全中心里看看是不是应用的“悬浮窗”的权限被禁止了安卓调试
   
 打开Chrome，访问 http://localhost:8081/debugger-ui ， 应当能看到一个页面。按F12打开开发者菜单。 在模拟器或真机菜单中选择Debug JS，即可开始调试。