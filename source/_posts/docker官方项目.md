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
   <!--more-->
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
    指定服务上执行一个命令。
    ```shell
     docker-compose run ubuntu ping docker.com
    ```
    默认会将相关联的(未启动)服务启动，但给定命令将会覆盖原有的自动运行命令，并且不会自动创建端口，以避免冲突。
    ```shell
     docker-compose run --no-deps web python manage.py shell #--no-deps 不启动关联的容器
    ```
       - -d 后台运行容器。
       - --name NAME 为容器指定一个名字。
       - --entrypoint CMD 覆盖默认的容器启动指令。
       - -e KEY=VAL 设置环境变量值，可多次使用选项来设置多个环境变量。
       - -u, --user="" 指定运行容器的用户名或者 uid。
       - --no-deps 不自动启动关联的服务容器。
       - --rm 运行命令后自动删除容器，d 模式下将忽略。
       - -p, --publish=[] 映射容器端口到本地主机。
       - --service-ports 配置服务端口并映射到本地主机。
       - -T 不分配伪 tty，意味着依赖 tty 的指令将无法运行。
  - scale
      
    设置指定服务运行的容器个数`docker-compose scale [options] [SERVICE=NUM...]`
    ```shell
      docker-compose scale web=3 db=2 #启动3个web服务，2个db服务
    ```
    > 当指定的服务大于运行中的容器，创建并启动新的容器，反之，停止多余的容器。
      
      - -t,--timeout TIMEOUT停止容器的超时(默认10s)

  - start
      
    启动已经存在的服务容器`docker-compose start [Service...]`
  - stop

    使运行状态的容器停止`docker-compose stop [optins] [SERVICE...]`
      - -t,--timeout TIMEOUT 
  - top
      
    查看各服务容器内运行的进程
  - unpause
     
    恢复处于暂停状态的服务`docker-compose unpause [SERVICE...]`
  - up

    `docker-compose up [options] [SERVICE...]`将尝试自动完成构建镜像，创建服务，启动服务，关联服务相关的容器，启动关闭的相关容器。默认`docker-compose up`启动的容器在前台，控制台会打印出所有容器的输出信息。使用`docker-compose up -d`将在后台启动并运行所有的容器。服务容器已经存在，`docker-compose up `将会尝试停止容器，然后重新创建（保持使用 `volumes-from `挂载的卷），以保证新启动的服务匹配 `docker-compose.yml` 文件的最新内容。如果用户不希望容器被停止并重新创建，可以使用 `docker-compose up --no-recreate`。这样将只会启动处于停止状态的容器，而忽略已经运行的服务。如果用户只想重新部署某个服务，可以使用 d`ocker-compose up --no-deps -d <SERVICE_NAME>` 来重新创建服务并后台停止旧服务，启动新服务，并不会影响到其所依赖的服务。
      - -d 在后台运行服务容器。
      - --no-color 不使用颜色来区分不同的服务的控制台输出。
      - --no-deps 不启动服务所链接的容器。
      - --force-recreate 强制重新创建容器，不能与 
      - --no-recreate 同时使用。
      - --no-recreate 如果容器已经存在了，则不重新创建，不能与 --force-recreate 同时使用。
      -  --no-build 不自动构建缺失的服务镜像。
      - -t, --timeout TIMEOUT 停止容器时候的超时（默认为 10 秒）。
  - version 

      打印版本信息 
### compose模板文件
  `compose`文件中的指令说明了构建的服务参数。
#### 指令详解
  - build 
     