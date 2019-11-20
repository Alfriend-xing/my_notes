# docker
Docker 从 1.13 版本之后采用时间线的方式作为版本号，分为社区版 CE 和企业版 EE，社区版是免费提供给个人开发者和小型团体使用的，企业版会提供额外的收费服务，比如经过官方测试认证过的基础设施、容器、插件等。

社区版按照 stable 和 edge 两种方式发布，每个季度更新 stable 版本，如 17.06，17.09；每个月份更新 edge 版本，如17.09，17.10。

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

# 列出本机正在运行的容器
docker container ls #container是容器管理工具，ls是命令之一
docker container ls --all
docker ps   #ps只是列出正在运行的容器
docker ps -a

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

## docker harbor

- https://www.jianshu.com/p/6fcacc2020d5
- https://juejin.im/post/5d9c2f25f265da5bbb1e3de5

## docker compose






