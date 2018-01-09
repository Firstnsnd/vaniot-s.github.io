---
title: 搭建Github Pages和Hexo博客
date: 2017-12-29 19:59:49
tags: Hexo
categories: web
---
### 一、安装配置Git
1.安装git,[下载地址](https://git-scm.com/downloads 'git')。
git中文教程[Pro git](http://git.oschina.net/progit/ )
2.配置用户信息：
```
$ git config --global user.name "vaniot"//用户名
$ git config --global user.email  "vaniot@gmail.com"//填写自己的邮箱
```
3.配置SSH密钥
配置Github的SSH密钥可以让本地git项目与远程的github建立联系，让我们在本地写了代码之后直接通过git操作就可以实现本地代码库与Github代码库同步。
打开git本机是否存在SSH Keys检测
```
$ cd ~/. ssh  //检测本机用户home目录下是否存在.ssh
```
若不存在此目录，创建一对新的SSH密钥
```
$ssh-keygen -t rsa -C "your_email@example.com"
#这将按照你提供的邮箱地址，创建一对密钥
#在此过程中会要求输入密码可选择不输入，及特定的文件名不输入则在默认的位置存储如/c/Users/you/.ssh/github_rsa
```
### 二、安装node.js及hexo在本地
[官网下载](https://nodejs.org/en/download/ "node.js")
安装完node后,打开git安装hexo命令工具
```
npm install hexo-cli -g #安装hexo命令行工具
```
创建Hexo文件夹,打开文件夹，打开文件夹使用git执行初始化
```
$ hexo init
$ npm install
```
为了避免出错安装以下插件
```
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked@0.2 --save
npm install hexo-renderer-stylus@0.2 --save
npm install hexo-generator-feed@1 --save
npm install hexo-generator-sitemap@1 --save
```
在本地查看效果:
```
hexo generate
hexo server
```
登录localhost:4000，即可看到本地的效果如下：
![初始化成功](https://raw.githubusercontent.com/Vaniot-s/picture/master/imagelocalhost.png)
### 三、Github Pages部署hexo 
GitHub是基于git实现的代码托管，可以免费使用，Github Pages可以被认为是用户编写的、托管在github上的静态网页，
简单快捷，使用Github Pages可以为你提供一个免费的服务器，免去了自己搭建服务器和写数据库的麻烦。
1.注册github
2.创建代码库
![创建代码库](https://raw.githubusercontent.com/Vaniot-s/picture/master/how-to-create-reposity.png)

ps：若要创建的博客地址与用户名相同则仓库名应与用户名相同即为:username.github.io
3.生成github pages
 进入仓库setting->github pages->choose a theme 
4.添加SSH Keys
进入个人主页Account Setting->SSH Keys将上述生成的SSH Keys的rsa_pub填入
![创建代码库](https://raw.githubusercontent.com/Vaniot-s/picture/master/SSH-github-SSH-OK.png)
5.测试
可以输入下面的命令，看看设置是否成功，git@github.com的部分不要修改：
```
$ ssh -T git@github.com
是下面的反馈：
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
输入yes就好，然后会看到：
Hi cnfeat! You've successfully authenticated, but GitHub does not provide shell access.
```
6.修改配置文件，部署到github
 打开Hexo文件找到_config.yml找到deploy修改：（确定已安装插件 npm install hexo-deployer-git --save）
 ```
 deploy:
  type: git
  repository: https://github.com/Vaniot-s/vaniot-s.github.io.git ##你的仓库地址
  branch: master
 ```
 生成静态文件及部署博客:
 ```
 hexo g -d
 ```
 7.在浏览器上输入自己的主页地址
 ![在线预览](https://raw.githubusercontent.com/Vaniot-s/picture/master/blog-main.png)
 ### 四、更换hexo主题
 1. 进入[Hexo主题官网](https://hexo.io/themes/)，选择喜欢的主题，进入到它的github地址，将其克隆到博客本地文件夹下的themes下：
  ```
 git colne  https://github.com/shenliyang/hexo-theme-snippet.git
  ```
2.打开Hexo文件夹下的配置文件_config.yml，修改参数为：theme: hexo-theme-next,返回Hexo目录，右键Git Bash，输入
```
hexo g
hexo s
```
3.打开浏览器，输入 http://localhost:4000/ 即可看见主题已经更换
部署到Github上
打开Hexo文件夹，右键Git Bash，输入
```
hexo clean   (必须要，不然有时因为缓存问题，服务器更新不了主题)
hexo g -d
```
### 五、Hexo写文章
1.Hexo支持[Markdown语法](http://blog.leanote.com/post/freewalk/Markdown-%E8%AF%AD%E6%B3%95%E6%89%8B%E5%86%8C#title-24)创建Markdown的文章
```
hexo new xxxxx
```
2.创建文章分类
scaffolds目录下，是新建页面的模板，执行新建命令时，是根据这里的模板页来完成的，所以可以在这里根据你自己的需求添加一些默认值。打开scaffolds/post.md文件，在tags:上面一行加入categories:,保存后，重新执行hexo n ‘name’命令，会发现新建的页面里有categories:项了
打开根目录下的配置文件_config.yml（注意不是themes主题目录下的_config.yml，那个文件叫 主题配置文件），找到如下位置做更改：
```
# Category & Tag
default_category: uncategorized
category_map:
	树莓派: raspberry
	写作: writing
	其他: others
tag_map:
```
ps:category_map: 是设置分类的地方，每行一个分类，冒号前面是分类名称，后面是访问路径.但不是每个主题均可以设置
### 六、配置评论
   开启Gitment评论，先去[这里](https://github.com/settings/applications/new) 注册一个新的 OAuth Application。其他内容可以随意填写，但要确保填入正确的 callback URL（一般是评论页面对应的域名，如 https://vaniot-s.github.io/ ）。 得到一个 client ID 和一个 client secret，这个将被用于之后的用户登录。打开主题的设置文件_config.ymal找到：主题评论
```
gitment:
  enable: true
  owner:   //github用户名
  repo:   //设置仓库
  client_id: 
  client_secret: 
  labels: 
  perPage: 
```
thanks to [@imsun](https://imsun.net)