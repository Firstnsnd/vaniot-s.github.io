---
title: git
date: 2017-12-30 00:21:22
tags: git
categories: tool
---
  git的常用操作的命令。
## git 版本的回退
  与版本回退的相关操作：
  ```shell
    git log #查看提交的日志
    git reset --hard HEAD^ #回退到上一个提交的版本
    git reset --hard HEAD~100 #往上回退100个版本
    git reset --hard commitId #回退到某个提交的coommitId
  ```
  <!--more-->
## 撤销修改
  将工作区的修改放弃
  ```shell
    git checkout --filname
  ```
## 克隆与推送到远程仓库
  - 克隆指定的分支到本地的仓库
    
    ```shell
      git clone -b branch remote Respository #指定克隆的分支
      git checkout SHA #指定历史版本号的tree的SHA
    ```
  - 跟踪远程分支
    
    ```shell
      # 查看所有的远程分支
      git ls-remote
      # 跟踪远程分支
      git checkout -b test origin/branchname #在本地创建test分支对应于远程的branchname
      #或者
      git checkout --track origin/branchname #将远程分支branchname拉取下来并设置本地的分支为branchname
      git branch -vv #查看跟踪的所有的分支
    ```
  - 将本地仓库推送
  
    将本地的仓库推送到远程，在本地与远程创建相同的仓库名，使用`git remote add origin remote Resporitory(远程仓库地址)`关联，使用`git push -u origin master`推送master分支的所有的内容。

## 分支的操作
  ```shell
    git branch #查看分支
    git branch branchname #创建分支
    git checkout branchname #切换分支
    git checkout -b branchname #创建并切换分支
    git merge branchname # 将分支和并到当前的分支上
    git branch -d branchname #删除分支
    git push origin --delete branchname #删除远程的分支
  ```
## git冲突
  - 多成员修改相同文件不同区域
  
    修改相同的文件内容后，提交冲突，在本地`git commit`后，使用`git stash`将内容暂存,再使用`git pull`从远程的仓库拉取,可看见冲突的文件,手动修改冲突的内容。
  - 同时修改文件名和文件内容
  - 删除冲突
    将删除的文件重新加入`git add`,或者删除`git rm`。
## 远程操作