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
docker run -it --rm ubuntu:18.04 bash #run 启动容器
```
<!--more-->
