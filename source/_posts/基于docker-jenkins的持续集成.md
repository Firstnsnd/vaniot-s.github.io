---
title: 基于docker+jenkins的持续集成
date: 2018-09-10 23:28:54
tags: [docker,jenjins]
categories: web
---
## 在docker中安装jenkins
拉取jenkins image
```shell
docker pull jenkins
```
Jenkins没有数据库，所有数据都是存放在文件中的，首先在本地创建Jenkins数据目录，用于保存Jenkins的数据 这个目录需要定期的备份，用于容灾（当前Jenkins容器所在节点由于不可抗因素无法使用时，可以在新机器上使用备份的数据启动新的jenkins master节点）。
```shell
sudo mkdir /var/jenkins
sudo chown 1000:1000 /var/jenkins
sudo docker run -p 8080:8080 -p 50000:50000 -v /var/jenkins:/var/jenkins_home --name my_jenkins -d jenkins
```