---
title: docker官方项目
date: 2018-09-03 14:45:23
tags: docker
categories: web
---
## Compose
   `Compose`定义和运行多个`Docker`容器的应用，实现对`Docker`容器集群的快速编排。通过一个`docker-compose.yml`模板文件，定义一组相关联的服务(容器应用)为一个项目(由多个服务组成的完整的业务单元)。
### Compose的使用
#### 命令
   `Compose`的命令对象默认指定为项目，使用 `docker-compose [COMMAND] --help`或`docker-compose help[COMMAND]`可以查看某个具体命令的使用格式。
   ```shell
    docker-compose [-f=<arg>...] [options] [COMMAND] [ARGS...]
   ```
#### 命令选项

  -  -f, --file FILE 指定使用的 Compose 模板文件，默认为 docker-compose.yml，可以多次指定。
  -  -p, --project-name NAME 指定项目名称，默认将使用所在目录名称作为项目名。
  -  --x-networking 使用 Docker 的可拔插网络后端特性
  - --x-network-driver DRIVER 指定网络后端的驱动，默认为 bridge
  -  --verbose 输出更多调试信息。
  - -v, --version 打印版本并退出。
#### 命令使用说明
  
  - build
  
    在项目目录下，(重新)构建项目中的容器:`docker-compose build [options] [SERVICE...]`服务容器构建后将会带上一个标记名。
        - --force-rm 删除构建过程中的临时容器
        - --no-cache 构建过程中不使用cache
        - --pull尝试通过pull获取更新版本的镜像
  - config
    
    验证Compose文件格式是否正确
  - down
   
    停止up命令所启动的容器，并删除网络
  - exec
   
    进入指定的容器
  - help
    
    获得一个命令的帮助
  - images

    列出Compose文件中包含的镜像
  - kill
    
    `docker-compose kill [options] [SERVICE...]`通过发送`SIGKILL`信号强制停止服务容器。带参数`-s`指定发送信号。
    ```shell
    docker-compose kill -s SIGINT
    ```
  
  - logs
   
    查看服务容器的输出，`docker-compose`对不同的服务输出使用不同的颜色区分，当带有`--no-color`关闭颜色

  - pause
   
    暂停一个服务容器,`docker-compose pause [SERVICE...]`
  - port 
    
    打印某个容器端口所映射的公共端口`docker-compose port [options] SERVICE PRIVATE_PORT`
     - -protocol=proto 指定端口协议，tcp(默认值)或者udp
     - --index=index 同一个服务存在多个容器，指定容器对象的序号
   - ps
     
     项目中的所有容器`docker-compose ps [options][SERVICE...]`
      - -q 只打印容器的ID信息
   - pull 
    
     拉取服务依赖的镜像`docker-compose pull [options][SERVICE...]`。
      - --ingore-pull-failure 忽略拉取取镜像过程中的错误
   - push
    
    推送服务依赖的镜像到docker镜像仓库
   - restart
    
    重启项目中的服务`docker-compose restart [options][SERVICE..]`
     - -t,-timeout TIMEOUT指定重启的超时时间(默认10秒) 

   - rm 

    删除(停止状态的容器)`docker-compose rm [options] [SERVICE...]`
      - -f,--force 强制直接删除，包括非停止状态的容器
      - -v 删除容器所挂载的数据卷
   - run
     
     在指定服务上执行命令`docker-compose run [options] [-p PORT..][-e KEY=VAL..] SERVICE [COMMAND] [ARGS...]`
    
    
    