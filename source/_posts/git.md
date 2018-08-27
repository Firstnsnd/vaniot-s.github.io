---
title: git
date: 2017-12-30 00:21:22
tags: git
categories: tool
---
## git 版本的回退
    与版本回退的相关操作：
    ‵‵‵shell
    git log #查看提交的日志
    git reset --hard HEAD^ #回退到上一个提交的版本
    git reset --hardHEAD~100 #往上回退100个版本
    git reset --hard commitId #回退到某个提交的coommitId
    ```
## 撤销修改
    将工作区的修改放弃
    ```shell
    git checkout --filname
    ```
## 推送到远程仓库
    在本地创建