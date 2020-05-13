# docker
 


## 安装
```shell
# 必要工具
yum install -y yum-utils device-mapper-persistent-data lvm2

# 添加源
# docker 官方源
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 阿里云源
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 解除版本默认的禁用(选用)
yum-config-manager --enable docker-ce-edge
yum-config-manager --enable docker-ce-test

# 安装
yum install docker-ce

# 启动
systemctl start docker

# 验证安装成功
docker run hello-world
```
## 镜像管理
```shell
docker search 镜像名        # 查找镜像
docker pull centos:latest  # 从docker.io中下载centos镜像到本地
docker images              # 查看已下载的镜像
docker rm image_id         # 删除镜像，指定镜像id
```
```shell
# 通过容器创建镜像，通过已存在的容器创建一个新的镜像
docker commit -m="描述信息" -a="镜像作者" 容器ID 镜像名:版本
```
```shell
# 通过Dockerfile创建镜像

touch Dockerfile    #创建镜像的参数
# 添加内容
FROM    centos:6.7  #指定使用哪个镜像源
COPY . /app         #复制文件，忽略ignore
WORKDIR /app        #切换工作路径
RUN     yum install -y wget #运行命令
EXPOSE  80          #开放容器80端口

touch .dockerignore #打包时忽略的文件(选做)
# 添加内容
.git
node_modules
npm-debug.log

# 创建镜像
docker build -t 镜像名:版本 .
# . 表示Dockerfile路径

```

## 容器管理
>容器就像一个类的实例
>docker run 相当于 docker create 加 docker start

```shell
# 创建容器并执行 echo "hello world"
docker run 镜像名[:版本] echo "hello world"
docker run centos yum install -y wget

# 创建一个新的容器但不启动
docker create [选项] 镜像名[:版本] [命令] [参数]

docker start [容器id/容器名] # 启动容器
docker stop [容器id/容器名]  # 停止容器
docker restart [容器id/容器名]  # 重启容器
docker kill [容器id/容器名]  # 杀死容器
docker rename [容器id/容器名]  # 重命名容器
docker rm [容器id/容器名]    # 删除容器
docker logs [容器id/容器名]  # 查看日志

docker exec -it [容器id/容器名] /bin/bash  # 进入容器
# exec表示在正在运行的容器中执行命令，这里为/bin/bash
# -it表示为执行的命令创建一个交互终端，使上面exec执行的bash可以交互

# 列出本机正在运行的容器
docker container ls #container是容器管理工具，ls是命令之一
docker container ls --all
docker ps   #ps只是列出正在运行的容器
docker ps -a

# 查看容器使用的资源
docker stats
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# 从容器拷贝到主机当前目录('.')
docker container cp [containID]:[/path/to/file] .
# 从主机复制到容器 
docker cp host_path containerID:container_path
```


## 命令
```shell
$ docker --help

管理命令:
  container   管理容器
  image       管理镜像
  network     管理网络
命令：
  attach      介入到一个正在运行的容器
  build       根据 Dockerfile 构建一个镜像
  commit      根据容器的更改创建一个新的镜像
  cp          在本地文件系统与容器中复制 文件/文件夹
  create      创建一个新容器
  exec        在正在运行的容器中执行一条命令
  images      列出镜像
  kill        杀死一个或多个正在运行的容器    
  logs        取得容器的日志
  pause       暂停一个或多个容器的所有进程
  ps          列出所有容器
  pull        拉取一个镜像或仓库到 registry
  push        推送一个镜像或仓库到 registry
  rename      重命名一个容器
  restart     重新启动一个或多个容器
  rm          删除一个或多个容器
  rmi         删除一个或多个镜像
  run         在一个新的容器中执行一条命令
  search      在 Docker Hub 中搜索镜像
  start       启动一个或多个已经停止运行的容器
  stats       显示一个容器的实时资源占用
  stop        停止一个或多个正在运行的容器
  tag         为镜像创建一个新的标签
  top         显示一个容器内的所有进程
  unpause     恢复一个或多个容器内所有被暂停的进程
```
## 命令参数

### run
```shell
-d  #后台运行
-p 主机端口:容器端口    #端口映射
--name="nginx-lb"       #指定容器名称
-m      #设置容器最大内存
-it     #进入终端
```

## 挂载本地目录
```shell
docker run -it -v /test:/soft centos /bin/bash
# 前面是主机路径，后面是容器路径，容器内会自动创建目录
```

## dockerfile

Docker可以通过Dockerfile自动生成自定义镜像。dockerfile文件内容包括 构建镜像所基于的镜像版本，向镜像中添加本地文件或网络文件，需要执行的命令(包括下载，编译，安装和一些文件操作等)，目录挂载，容器主进程的启动命令。

```dockerfile
# 指定基础镜像
FROM <image>

# 设置环境变量，使用时加$
ENV <key> <value>
ENV <key1>=<value1> <key2>=<value2>
ENV VERSION=1.0 DEBUG=on NAME="Happy Feet"

# 执行命令
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
         && apt-get purge -y --auto-remove $buildDeps

# 复制文本
COPY <源路径>... <目标路径>

# 高级复制文件，<源路径> 可以是一个 URL
ADD <源路径>... <目标路径>

# 构建参数
ARG user1=someuser
ARG buildno=1
# 构建时可使用--build-arg <参数名>=<值> 覆盖默认值
# 构建参数和ENV的效果一样，都是设置环境变量。不同的是，ARG所设置的构建环境的环境变量在将来容器运行时是不会存在的。

# 定义匿名卷，指定后容器向这些路径的写入操作会进入外部匿名卷中
# 挂载外部指定卷使用 docker run -d -v mydata:/data xxxx 命令
VOLUME ["<路径1>", "<路径2>"...]
VOLUME <路径>

# 暴露端口
# 仅声明容器打算使用的端口，并不会自动在宿主进行端口映射。
EXPOSE <端口1> [<端口2>...] 

# 指定工作目录
WORKDIR <工作目录路径>
RUN cd /app
RUN echo "hello" > world.txt
# 两次run不在一个环境内，可以使用WORKDIR。

# 指定当前用户
# 这个用户必须是事先建立好的，否则无法切换。
USER <用户名>

# 健康检查
HEALTHCHECK [选项] CMD <命令> 
  可选选项
    --interval=<间隔> ：两次健康检查的间隔，默认为 30 秒；
    --timeout=<时长> ：健康检查命令运行超时时间，如果超过这个时间，本次健康检查就被视为失败，默认 30 秒；
    --retries=<次数> ：当连续失败指定次数后，则将容器状态视为 unhealthy ，默认3次。
    --start-period=DURATION：启动时间，默认 0s。为需要启动的容器提供了初始化的时间段， 在这个时间段内如果检查失败， 则不会记录失败次数。 如果在启动时间内成功执行了健康检查， 则容器将被视为已经启动， 如果在启动时间内再次出现检查失败， 则会记录失败次数。
  命令可以是shell脚本的命令或者exec数组，命令返回值为：0 health状态，1 unhealth状态，2 reserved状态
# 禁止从父镜像继承的HEALTHCHECK生效 
HEALTHCHECK NODE 

# 容器启动命令
# 未定义ENTRYPOINT时，启动容器命令后接的参数会覆盖cmd执行
CMD ["nginx", "-g", "daemon off;"]

# 入口点,指定容器启动的参数
# 如果指定了ENTRYPOINT指令，CMD就不再是直接运行了，而是将CMD的内容作为参数传递给ENTRYPOINT指令。如果启动容器命令后接了参数会覆盖cmd传给ENTRYPOINT。
# 覆盖ENTRYPOINT使用 docker run的参数-entrypoint
ENTRYPOINT shell 或 exec数组

```

构建镜像

- 当前目录：`docker build .`
- 指定路径：`docker build -f /path/to/a/Dockerfile .`
- 其他命令：`-m` `-t` `--cpus` 等

## docker compose

Compose 是 docker 提供的一个命令行工具，用来定义和运行由多个容器组成的应用。

- linux平台安装 

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# $(uname -s) -s表示Linux
# $(uname -m) -m表示x86_64

sudo chmod +x /usr/local/bin/docker-compose
```


```yaml
version: '3'
services:
  web:
    build: .
    image: xximage:0.1.13
    ports:
      - "5000:5000"
    volumes:
      - .:/code
    environment:
      FLASK_ENV: development
  redis:
    image: "redis:alpine"
```
- version表示配置文件使用的语法版本
- services定义了若干个服务，每个服务需要启动一个容器，这里有两个服务web和redis
- build表示构建镜像使用的dockerfile路径
- image表示使用已存在的镜像，如果同时指定build和images会构建镜像并且把镜像命名为image后面的那个名字
- ports表示端口映射
- volumes表示挂载卷，冒号前为外部路径(相对，绝对)，之后为容器内路径(绝对)
- environment表示环境变量


指定dcoker-compose文件 
- `docker-compose -f docker-compose-dev.yml up`

dcoker-compose命令
- 启动 `docker-compose up`
- 启动多个相同服务 `docker-compose up -d --scale redis=2`
- 单独启动部分服务 `docker-compose run <service> [<service>] ...`
- 停止所有服务 `docker-compose stop`
- 停止所有服务并删除容器和创建的镜像 `docker-compose down`，加 `--volumes` 同时删除挂载文件;如果是前端运行直接按 `ctrl+c`
- 删除现有的容器并且重新创建新的容器 `docker-compose up -d --force-recreate`
- 重启服务 `docker-compose restart [<service>]`
- 合并多个compose文件 `docker-compose -f docker-compose-base.yml -f docker-compose-dev.yml config`
  - config 命令不会执行真正的操作，而是显示 docker-compose 程序解析到的配置文件内容

[常用命令](https://www.cnblogs.com/moxiaoan/p/9299404.html)

```shell
docker-compose up -d nginx  构建建启动nignx容器
docker-compose exec nginx bash  登录到nginx容器中
docker-compose down 删除所有nginx容器,镜像
docker-compose ps 显示所有容器
docker-compose restart nginx  重新启动nginx容器
docker-compose run --no-deps --rm php-fpm php -v  在php-fpm中不启动关联容器，并容器执行php -v 执行完成后删除容器
docker-compose build nginx  构建镜像 。 
docker-compose build --no-cache nginx 不带缓存的构建。
docker-compose logs  nginx  查看nginx的日志 
docker-compose logs -f nginx  查看nginx的实时日志
docker-compose config  -q 验证（docker-compose.yml）文件配置，当配置正确时，不输出任何内容，当文件配置错误，输出错误信息。 
docker-compose events --json nginx  以json的形式输出nginx的docker日志
docker-compose pause nginx    暂停nignx容器
docker-compose unpause nginx  恢复ningx容器
docker-compose rm nginx 删除容器（删除前必须关闭容器）
docker-compose stop nginx 停止nignx容器
docker-compose start nginx   启动nignx容器
```

### 使用 network

Docker 提供的 network 功能能够对容器进行网络上的隔离

```yaml
version: '3'
services:
  proxy:
    image: nginx
    ports:
      - "80:80"
    networks:
      - frantnet
  webapp:
    build: .
    networks:
      - frantnet
      - endnet
  redis:
    image: redis
    networks:
      - endnet
networks:
  frantnet:
  endnet:
```

在yml文件中设置新的自定义网段,二级参数是在compose中自己设置的网段别名,这个别名可供services设置使用

```yaml
networks:
  networktest2:
     driver: bridge
     ipam:
       driver: default
       config:
          -
           subnet: 192.168.100.0/24
```


### 使用数据卷

```yaml
version: "{{ site.compose_file_v2 }}"
services:
  web:
    build: .
    ports:
     - "5000:5000"
    volumes:
     - .:/code
    networks:
      - front-tier
      - back-tier
  redis:
    image: redis
    volumes:
      - redis-data:/var/lib/redis
    networks:
      - back-tier
volumes:
  redis-data:
    driver: local
networks:
  front-tier:
    driver: bridge
  back-tier:
    driver: bridge
```




## 私有镜像库`harbor` 

用于管理docker镜像

- https://www.jianshu.com/p/6fcacc2020d5
- https://juejin.im/post/5d9c2f25f265da5bbb1e3de5






