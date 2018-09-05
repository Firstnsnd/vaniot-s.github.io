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

    指定`Dockerfile`的文件路径(绝对路径或相对`docker-compose.yml`文件路径)  
    ```shell
    # 用build指定目录
    version: '3'
    services:
    webapp:
        build: ./dir

    # 
    version: '3'
    services:
    webapp:
        build:
            context: ./dir # 使用context指令指定`Dockerfile`所在文件夹的路径
            dockerfile:Dockerfile-alternate # 指定Dockerfile的文件名
            args:
                buildno: 1 # 指令指定构建镜像时的变量

    # 使用cache_from 指定构建镜像的缓存
    build:
        context: .
        cache_from:
            - alpine:latest
            - corp/web_app:3.14
    ```
  - cap_add,cap_drop

    指定容器的内和能力(capacity)分配。
    ```shell
    cap_add: # 容器拥有所有的能力
     - ALL
    cap_drop: #去掉NET_ADMIN能力
     - NET_ADMIN
    ```
  - command

    覆盖容器启动后默认执行的命令
    ```shell
    command: echo "hello world"
    ```  
  - configs

    仅用于`Swarm mode`,

  - cgroup_parent
   
    指定父`cgroup`组,继承该组的资源限制.
    ```shell
    cgroup_parent:cgroup_1
    ```
  - container_name
    指定容器名称，默认会使用`项目名称_服务名称_序号`的格式
    ```shell
    container_name:docker-web-container
    ```
    > 指定容器名称后，该服务将无法进行扩展（scale），Docker 中不允许多个容器具有相同的名称。
  
  - deploy

    仅用于 Swarm mode
  - devices

    指定设备映射关系。
    ```shell
    devices:
    - "/dev/ttyUSB1:/dev/ttyUSB0"
    ```
  - depends_on

    解决容器的依赖、启动先后的问题。以下例子中会先启动 redis db 再启动 web
    ```shell
    version: '3'
    services:
    web:
        build: .
        depends_on:
        - db
        - redis
    redis:
        image: redis
    db:
        image: postgres
    ```
    > 注意：web 服务不会等待 redis db 「完全启动」之后才启动。

  - dns

    自定义 DNS 服务器。可以是一个值，也可以是一个列表。
    ```
    dns: 8.8.8.8
    dns:
    - 8.8.8.8
    - 114.114.114.114
    ```
  - dns_search

    配置 DNS 搜索域。可以是一个值，也可以是一个列表。
    ```shell
    dns_search: example.com
    dns_search:
    - domain1.example.com
    - domain2.example.com
    ```
  - tmpfs

    挂载一个 tmpfs 文件系统到容器。
    ```shell
    tmpfs: /run
    tmpfs:
    - /run
    - /tmp
    ```
  - env_file

    从文件中获取环境变量，可以为单独的文件路径或列表。

    如果通过 docker-compose -f FILE 方式来指定 Compose 模板文件，则 env_file 中变量的路径会基于模板文件路径。

    如果有变量名称与 environment 指令冲突，则按照惯例，以后者为准。
    ```shell
    env_file: .env

    env_file:
    - ./common.env
    - ./apps/web.env
    - /opt/secrets.env
    ```
    环境变量文件中每一行必须符合格式，支持 # 开头的注释行。
    ```shell
    # common.env: Set development environment
    PROG_ENV=development
    ```
  - environment

    设置环境
    配置日志选项。组或字典两种格式。

    只给定名
    配置日志选项。运行 Compose 
    配置日志选项。，可以用来防止泄露不
    配置日志选项。
    ```shell
    配置日志选项。
    environment:
    RACK_ENV: development
    SESSION_SECRET:
    environment:
    - RACK_ENV=development
    - SESSION_SECRET
    ```
    如果变量名称或者值中用到 true|false，yes|no 等表达 布尔 含义的词汇，最好放到引号里，避免 YAML 自动解析某些内容为对应的布尔语义。这些特定词汇，包括
    ```shell
    y|Y|yes|Yes|YES|n|N|no|No|NO|true|True|TRUE|false|False|on|On|ON|off|Off|OFF
    ```
  - expose

    暴露端口，但不映射到宿主机    配置日志选项。连接的服务访问。

    仅可以指定内部端口为参数
    ```shell
    expose:
    - "3000"
    - "8000"
    ```
  - external_links

    >注意：不建议使用该指令。

    链接到 docker-compose.yml 外部的容器，甚至并非 Compose 管理的外部容器。
    ```shell
    external_links:
    - redis_1
    - project_db_1:mysql
    - project_db_1:postgresql
    ```
  - extra_hosts

    类似 Docker 中的 --add-host 参数，指定额外的 host 名称映射信息。
    ```shell
    extra_hosts:
    - "googledns:8.8.8.8"
    - "dockerhub:52.1.157.61"
    ```
    会在启动后的服务容器中 /etc/hosts 文件中添加如下两条条目。
    ```shell
    8.8.8.8 googledns
    52.1.157.61 dockerhub
    ```
  - healthcheck

    通过命令检查容器是否健康运行。
    ```shell
    healthcheck:
    test: ["CMD", "curl", "-f", "http://localhost"]
    interval: 1m30s
    timeout: 10s
    retries: 3
    ```
  - image

    指定为镜像名称或镜像 ID。如果镜像在本地不存在，Compose 将会尝试拉取这个镜像。
    ```shell
    image: ubuntu
    image: orchardup/postgresql
    image: a4bc65fd
    ```
  - labels

    为容器添加 Docker 元数据（metadata）信息。例如可以为容器添加辅助说明信息。
    ```shell
    labels:
    com.startupteam.description: "webapp for a startup team"
    com.startupteam.department: "devops department"
    com.startupteam.release: "rc3 for v1.0"
    ```
  - links

    >不推荐使用该指令。

  - logging
    配置日志选项。
    ```shell
    logging:
    driver: syslog
    options:
        syslog-address: "tcp://192.168.0.42:123"
    ```
    目前支持三种日志驱动类型。
    ```shell
    driver: "json-file"
    driver: "syslog"
    driver: "none"
    ```
    options 配置日志驱动的相关参数。
    ```shell
    options:
    max-size: "200k"
    max-file: "10"
    ```
  - network_mode

    设置网络模式。使用和 docker run 的 --network 参数一样的值。
    ```shell
    network_mode: "bridge"
    network_mode: "host"
    network_mode: "none"
    network_mode: "service:[service name]"
    network_mode: "container:[container name/id]"
    ```
  - networks

    配置容器连接的网络。
    ```shell
    version: "3"
    services:

    some-service:
        networks:
        - some-network
        - other-network

    networks:
    some-network:
    other-network:
    ```
  - pid

    跟主机系统共享进程命名空间。打开该选项的容器之间，以及容器和宿主机系统之间可以通过进程 ID 来相互访问和操作。
    ```shell
    pid: "host"
    ```
  - ports

    暴露端口信息。

    使用宿主端口：容器端口 (HOST:CONTAINER) 格式，或者仅仅指定容器的端口（宿主将会随机选择端口）都可以。
    ```shell
    ports:
    - "3000"
    - "8000:8000"
    - "49100:22"
    - "127.0.0.1:8001:8001"
    ```
    注意：当使用 HOST:CONTAINER 格式来映射端口时，如果你使用的容器端口小于 60 并且没放到引号里，可能会得到错误结果，因为 YAML 会自动解析 xx:yy 这种数字格式为 60 进制。为避免出现这种问题，建议数字串都采用引号包括起来的字符串格式。
  - secrets

    存储敏感数据，例如 mysql 服务密码。
    ```shell
    version: "3.1"
    services:

    mysql:
    image: mysql
    environment:
        MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
    secrets:
        - db_root_password
        - my_other_secret

    secrets:
    my_secret:
        file: ./my_secret.txt
    my_other_secret:
        external: true
    ```
  - security_opt

    指定容器模板标签（label）机制的默认属性（用户、角色、类型、级别等）。例如配置标签的用户名和角色名。
    ```shell
    security_opt:
        - label:user:USER
        - label:role:ROLE
    ```
  - stop_signal

    设置另一个信号来停止容器。在默认情况下使用的是 SIGTERM 停止容器。
    ```shell
    stop_signal: SIGUSR1
    ```
  - sysctls

    配置容器内核参数。
    ```shell
    sysctls:
    net.core.somaxconn: 1024
    net.ipv4.tcp_syncookies: 0

    sysctls:
    - net.core.somaxconn=1024
    - net.ipv4.tcp_syncookies=0
    ```
  - ulimits

    指定容器的 ulimits 限制值。

    例如，指定最大进程数为 65535，指定文件句柄数为 20000（软限制，应用可以随时修改，不能超过硬限制） 和 40000（系统硬限制，只能 root 用户提高）。

    ```shell
    ulimits:
        nproc: 65535
        nofile:
        soft: 20000
        hard: 40000
    ```
  - volumes

    数据卷所挂载路径设置。可以设置宿主机路径 （HOST:CONTAINER） 或加上访问模式 （HOST:CONTAINER:ro）。

    该指令中路径支持相对路径。
    ```shell
    volumes:
    - /var/lib/mysql
    - cache/:/tmp/cache
    - ~/configs:/etc/configs/:ro
    ```
  - 其它指令

    此外，还有包括 `domainname, entrypoint, hostname, ipc, mac_address, privileged, read_only, shm_size, restart, stdin_open, tty, user, working_dir` 等指令，基本跟 docker run 中对应参数的功能一致。

    指定服务容器启动后执行的入口文件。
    ```shell
    entrypoint: /code/entrypoint.sh
    ```
    指定容器中运行应用的用户名。
    ```shell
    user: nginx
    ```
    指定容器中工作目录。
    ```shell
    working_dir: /code
    ```
    指定容器中搜索域名、主机名、mac 地址等。
    ```shell
    domainname: your_website.com
    hostname: test
    mac_address: 08-00-27-00-0C-0A
    ```
    允许容器中运行一些特权命令。
    ```shell
    privileged: true
    ```
    指定容器退出后的重启策略为始终重启。该命令对保持服务始终运行十分有效，在生产环境中推荐配置为 always 或者 unless-stopped。
    ```shell
    restart: always
    ```
    以只读模式挂载容器的 root 文件系统，意味着不能对容器内容进行修改。
    ```shell
    read_only: true
    ```
    打开标准输入，可以接受外部输入。
    ```shell
    stdin_open: true
    ```
    模拟一个伪终端。
    ```shell
    tty: true
    ```
  - 读取变量

    Compose 模板文件支持动态读取主机的系统环境变量和当前目录下的 .env 文件中的变量。

    例如，下面的 Compose 文件将从运行它的环境中读取变量 ${MONGO_VERSION} 的值，并写入执行的指令中。
    ```shell
    version: "3"
    services:

    db:
    image: "mongo:${MONGO_VERSION}"
    ```
    如果执行 MONGO_VERSION=3.2 docker-compose up 则会启动一个 mongo:3.2 镜像的容器；如果执行 MONGO_VERSION=2.8MONGO_VERSION=2.8 docker-compose up 则会启动一个 mongo:2.8 镜像的容器。
    在当前目录新建 .env 文件并写入以下内容。
    ```
    # 支持 # 号注释
    MONGO_VERSION=3.6
    ```
    执行 docker-compose up 则会启动一个 mongo:3.6 镜像的容器。
## Machine
  
## Swarm
  `Docker Swarm`提供`Docker`容器集群服务，利用`Docker Swarm`可以将多个`Docker`主机封装为单个大型的虚拟主机，Docker 1.12 Swarm mode 已经内嵌入 Docker 引擎，成为了 docker 子命令 docker swarm。
### 节点
### 服务及任务
