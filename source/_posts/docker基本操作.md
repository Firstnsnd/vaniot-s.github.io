---
title: docker基本操作
date: 2018-06-21 19:50:27
tags: docker
categories: web
---
## 镜像
### 获取镜像
docker获取镜像的命令：
```shell
docker pull --help #查看docker 拉取镜像的的命令格式
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签] #默认的仓库地址为Docker Hub,仓库名为两段式名称，即 <用户名>/<软件名>

docker pull ubuntu:18.04
```
### 运行
已获取的镜像，作为容器的基础启动
```shell
docker run -it --rm --name ubuntu ubuntu:18.04 bash #run 启动容器 -i 交互操作 -t 终端 --rm 容器退出后会立即删除 --name 命名为ubuntu bash进入shell
```
<!--more-->
### 查看镜像
- 列出已经下载的镜像
```shell
docker image ls # 列表包含了 仓库名、标签、镜像 ID、创建时间 以及 所占用的空间
```
> 镜像 ID 则是镜像的唯一标识，一个镜像可以对应多个标签

- 查看镜像的体积
```shell
docker system shell  #查看镜像、容器、数据卷所占用的空间
```
镜像仓库中显示的为压缩后的体积，远大于本地的镜像的体积，除此之外，镜像是多层存储结构，可以继承，复用，不同的镜像使用相同的镜像，故拥有共同的层，实际镜像的占用空间或许会比列表中的占用要小。

- 虚悬镜像
当下载已存在的镜像时，原有的镜像的名会被转移到新的镜像，旧有的镜像仓库名、标签均为`<none>`,被称做为虚悬镜像(dangling image)
```shell
docker image ls -f dangling=true #只查看虚悬镜像
docker image prune #删除虚悬镜像
```
- 中间层镜像
docker的中间层镜像，是为了加速镜像的构建，重复利用资源，中间层镜像也没有标签，会随着依赖它的镜像删除而被删除。
```shell
docker image ls -a # 查看中间层镜像
```
- 镜像过滤(列出部分镜像) 
依据条件列出镜像 --filter(简写为:-f)过滤器参数
```shell
docker image ls ubuntu # 根据仓库名列出镜像
docker image ls ubuntu:18.04 #根据指定的仓库名和标签列出镜像
docker image ls -f since=ubnutu:18.04 #列出在ubuntu：18.04之后创建的镜像
docker image lf -f before=ubuntu:18.04 # 列出在ubuntu:18.04之前创建的镜像
docker image ls -f label=test #根据Label 过滤，如果镜像创建时定义了
```
- 特定的格式显示
利用参数筛选出镜像的特定的信息,或使用go的模板语法[go的模板语法](https://gohugo.io/templates/introduction/) 指定显示的镜像的信息。
```shell
docker image ls  -q # 只显示镜像的id
docker image ls  --format "{{.ID}]: {{.Repository}}" #直接列出镜像结果，并且只包含镜像ID和仓库名
docker imagels --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}" #以表格等距显示，并且有标题行，和默认一样，自己定义列
```
### 删除镜像
#### 删除本地镜像
```shell
docker image rm --help #查看删除的命令参数
docker image rm [OPTIONS] IMAGE [IMAGE...] 
#Aliases: rm, rmi, remove
#Options: 
#   -f, --force Force removal of the image
#   --help       Print usage 
#   --no-prune   Do not delete untagged parents
docker image rm 501 #使用镜像的短id删除镜像
docker image rm centos #使用镜像名(<仓库名>:<标签>)删除镜像
docker image ls --digests #查看镜像并列出摘要
docker image rm  node@sha256:b4f0e0bdeb578043c1ea6862f0d40cc4afe32a4a582f3be235a3b164422be228 #使用镜像摘要删除镜像
``` 
#### Untagged 和 Deleted
删除标签镜像时，首先会将目标镜像的标签取消，当还有其他的标签指向该镜像时，并不会执行Delete操作。当镜像的层被其他镜像依赖，或有以该镜像为基础的容器，均不会触发Delete操作。
#### 使用docker image ls 配合删除
根据查询的结果成批的删除镜像列表
```shell
docker image rm $(docker image ls -q redis)
docker image rm $(docker image ls -q -f before =mongo:3.2)
```
> CentOS/RHEL 的用户需要注意的事项 ??? [详见](https://yeasy.gitbooks.io/docker_practice/content/image/rm.html#untagged-%E5%92%8C-deleted)

### 镜像的构成
docker可用于定制镜像，但一般不用于定制镜像，`docker commit`会将上一层的镜像跟随当前的存储成而变得臃肿，`docker commit`生成的尽享对于其他是黑箱操作，不可重复。`docker commit`一般用于入侵后的现场的保护。
``` shell
docker run --name webserver -d -p 8099:80 nginx
#启动 nginx 容器并将其映射到本地的8099端口 用浏览器打开http:localhost:8099 输出 welcome to Nginx
docker exec -it webserver bash #交互的方式进入容器

#修改页面的内容
root@b406af708fb8:/# echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
root@b406af708fb8:/# exit
#用浏览器打开http:localhost:8099 输出 Hello Docker
docker diff webserver #查看具体的改动
docker commit --help #查看 commit 的参数
#	docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
# Create a new image from a container's changes
# Options:
#   -a, --author string    Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
#   -c, --change list      Apply Dockerfile instruction to the created image (default [])
#       --help             Print usage
#   -m, --message string   Commit message
#   -p, --pause            Pause container during commit (default true)

docker commit -a "vaniot a developer" -m "change the content of index.html" webserver nginx:v2.0 #提交生成新的镜像
docker history nginx:v2.0 #查看nginx:v2.0的变化
```
### Dockerfie定制镜像
#### Dockerfile概述
  Dockerfile是一个文本文件，包含了许多的指令，每一个指令都将会建立一层。将需要定制的镜像的每一层修改，安装，构建，操作的命令都写入其中。解决重复构建、构建的透明性及体积。
#### 从dcoker引擎中获取
- FROM 指定基础镜像

  `FROM` 为`Dockerfile`指定了镜像的基础，之后的操作均在其的基础之上进行。
  > Docker中存在一个空白的镜像名为`scratch`,以此镜像为基础，则说明之后的指令将作为镜像的第一层开始。
  ```shell
  FROM scratch
  ```
- RUN 执行命令

  RUN用于执行命令的两种格式：

    * shell:`RUN<命令>`,类似与命令行中输入命令
      ```shell
      RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
      ```
    * exec: RUN ["可执行文件", "参数1", "参数2"]
   
  ><b>ps:</b> 每一个RUN都会创建一层，对于一层中不使用多个`RUN`，Dockerfile 支持 Shell 类的行尾添加 \ 的命令换行方式，以及行首 # 进行注释的格式。良好的格式，比如换行、缩进、注释等，会让维护、排障更为容易。镜像构建时，一定要确保每一层只添加真正需要添加的东西，任何无关的东西都应该清理掉.

  ```shell
  FROM debian:jessie

  RUN buildDeps='gcc libc6-dev make' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps #执行清理工作
  ```

  <i>example:</i> 构建nginx
  ```shell 
    mkdir nginx #构建空的文件夹
    cd nginx 
    touch Dockerfile 
    #在Dockerfile中写入
    FROM nginx
    RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
    #构建镜像
    docker build -t nginx:v3 . #构建镜像 `.`不能缺少
    docker run  -d -p 8099:80 nginx:v3.0 #创建容器
  ```
  > 在docker中`.`指定了上下文， 对于'docker build'的原理，Docker 运行是分为Docker引擎(服务端守护进程)和客户端工具。Docker 的引擎提供了一组 REST API([ Docker Remote API](https://docs.docker.com/develop/sdk/)),客户端工具通过API与Docker引擎交互。使用的远程调用形式在服务端（Docker 引擎）完成。`docker build `命令在服务端构建镜像。用户会指定构建镜像上下文的路径，docker build 命令得知这个路径后，会将路径下的所有内容打包，然后上传给 Docker 引擎。这样 Docker 引擎收到这个上下文包后，展开就会获得构建镜像所需的一切文件。

#### 从Git repo中构建
  ```shell
   docker build https://github.com/twang2218/gitlab-ce-zh.git#:8.14  
  ```
  指定构建所需的 Git repo，并且指定默认的 master 分支，构建目录为 /8.14/，然后 Docker 自己去 git clone 这个项目、切换到指定分支、并进入到指定目录后开始构建。
#### 使用 tar压缩包构建
  ```shell
   docker build http://server/context.tar.gz
  ```
  Docker 引擎会下载这个包，并自动解压缩，以其作为上下文，开始构建。
#### 标准输入中读取 Dockerfile 进行构建
  ```shell
   docker build - < Dockerfile # cat Dockerfile | docker build -
  ```
#### 标准输入中读取上下文压缩包进行构建
  ```shell
   docker build - < context.tar.gz
  ```
#### Dockerfile指令详解
- Copy复制文件
  
  格式有如下两种:
  
   - COPY <源路径> ... <目标路径>(类似于命令行)
   - COPY ["<源路径1>",... "<目标路径>"] (类似于函数调用)
  `<源路径>`可以是多个或者满足Go`[filepath.Match](https://golang.org/pkg/path/filepath/#Match)`规则的通配符，`<目标路径>` 可以是容器内的绝对路径，也可以是相对于工作目录的相对路径（工作目录可以用 WORKDIR 指令来指定）。目标路径不需要事先创建，如果目录不存在会在复制文件前先行创建缺失目录。
  ```shell
    COPY package.json /usr/src/app/ #将构建上下文目录中<源路径>的文件/目录复制到新一层的镜像内的<目标路径>位置
    COPY hom* /mydir/
    COPY hom?.txt /mydir/
  ```
  > 使用 `COPY` 指令，源文件的各种元数据都会保留。比如读、写、执行权限、文件变更时间等。
- ADD 复制文件

  `ADD` 和 `COPY` 的格式和性质基本一致，`ADD`可执行更加复杂的更能，`ADD`允许`<源路径>`为`URL`，`Docker`引擎会下载该链接的文件到`<目标文件>`并将权限设置为600(若要修改权限需要额外的加一层`RUN`进行权限的修改),对于下载的压缩包，解压也需要一层`RUN`指令进行解压(`<源路径>`为一个 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，ADD 指令将会自动解压缩这个压缩文件到`<目标路径>`去)
  ```shell
   FROM scratch
   ADD ubuntu-xenial-core-cloudimg-amd64-root.tar.gz / #自动解压ubuntu镜像
  ```

  > ADD 指令会令镜像构建缓存失效，所有的文件复制均使用 COPY 指令，仅在需要自动解压缩的场合使用 ADD。

- CMD 容器的启动

  容器是一个进程，容器中的应用均应该以<b>前台执行</b>，`CMD`用于指定默认的容器主进程的启动命令。`CMD`的两种格式:

  - shell：CMD `<命令>`
  - exec : CMD [“可执行文件”,"参数1","参数2",...] #解析时会被解析为JSON数组，使用时必须为双引号。
  ```shell
    #Dockerfile
    FROM ubuntu:16.04
    RUN apt-get update \
      && apt-get install -y curl \
      && rm -rf /var/lib/apt/lists/*
    CMD [ "curl", "-s", "http://ip.cn" ]
    #build 
    docker build -t myip 。
    #run 
    docker run myip #输出当前IP的地址
    docker run myip curl -s http://ip.cn -i #覆盖掉之前的CMD 增加http头信息
  ```
- ENTRYPOINT 

  `ENTRYPOINT`指定容器启动程序及参数，也可通过在`docker run`时指定参数`--entrypoint`修改已经指定的`EMTRYPOINT`容器启动程序及参数。
  ```shell
  #Dockerfile
  FROM ubuntu:16.04
  RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
  ENTRYPOINT [ "curl", "-s", "http://ip.cn" ]
  #build 
  docker build  -t ip .
  #run 
  docker run ip
  docker run it -i #修改ENTRYPOINT的内容
  ```
- ENV环境变量
  `ENV`设置环境变量，其值可在其他指令中使用,设置环境变量的格式如下：
   
    - ENV <key><value>
    - ENV <key1>=<value1> <key2>=<value2>...
  ```shell
  ENV NODE_VERSION 8.11.4
  RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-x64.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 \
  && rm "node-v$NODE_VERSION-linux-x64.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs
  ```
- ARG 
> **`*`** [Dockerfile 最佳实践文档 ](https://yeasy.gitbooks.io/docker_practice/appendix/best_practices.html)






























> 根据[docker practice](https://yeasy.gitbooks.io/docker_practice/content/introduction/)整理而来。