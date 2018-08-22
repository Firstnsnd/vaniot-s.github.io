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
docker run -it --rm ubuntu:18.04 bash #run 启动容器 -i 交互操作 -t 终端 --rm 容器退出后会立即删除 bash进入shell
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
删除本地镜像
```shell
docker image rm --help #查看删除的命令参数
docker image rm [OPTIONS] IMAGE [IMAGE...] 
#Aliases: rm, rmi, remove
#Options: 
#   -f, --force Force removal of the image
#   --help       Print usage 
#   --no-prune   Do not delete untagged parents
```
*根据[docker practice](https://yeasy.gitbooks.io/docker_practice/content/introduction/)整理而来