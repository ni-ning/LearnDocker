# Docker

### 基本概念

**NameSpace 资源隔离**
- PID 进程编号
- NET 网络设备、网络协议栈、端口等
- IPC 信号量、消息队列、共享内存
- MOUNT 文件系统、挂载点
- UTS 主机名和主机域
- USER 操作进程的用户和用户组

```
Build Once, Run Anywhere
               -- Solomon Hykes
```

**What is Docker 是什么**
- Docker基于容器技术的轻量级虚拟化解决方案
- Docker是容器引擎，把Linux的cgroup、namespace等容器底层技术进行封装抽象为用户提供了创建和管理容器的便捷界面(包括命令行和API)
- Docker 开源项目，基于Google公司推出的Go语言实现
- Docker引入了一整套容器管理的生态，包括分层的镜像模型、容器注册库、友好的Rest API


### 安装与基本命令

- 安装 [Ubuntu for Docker CE](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
- [建立 docker 用户组](https://yeasy.gitbooks.io/docker_practice/install/ubuntu.html)

```

vim /etc/docker/daemon.json
{
    "graph": "/data/docker",
    "storage-driver": "overlay2",
    "insequre-registries": [],
    "registry-mirrors": [],
    "bip": "172.7.5.1/24",
    "exec-opts": ["native.cgroupdriver=systemd"],
    "live-restore": true
}

systemctl enable docker
systemctl start docker

docker info
docker run hello-world

docker login
docker logout
cat /home/ubuntu/.docker/config.json

docker search alpine
docker pull alpine:latest
docker images | grep alpine
# docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
docker tag apline:latest nining1314/apline:v1.0
docker push nining1314/alpine:v1.0

docker rmi ${image_id} | docker rmi ${image_name}
```



### 镜像

Docker 镜像特性


`docker image rm redis`
```
容器是以镜像为基础，再加一层存储层，组成这样的多层存储结构去运行 -> 检查镜像的容器(即使容器没有运行)，存在时则不可删除镜像

镜像的唯一标识是其 ID 和摘要，而一个镜像可以有多个标签 ->  先将满足条件镜像标签都取消 Untagged；删除了所指定的标签后，执行 Deleted

镜像是多层存储结构 -> 从上层向基础层方向依次进行判断删除
```

### docker container --help

当利用 docker run 来创建容器时，Docker 在后台运行的标准操作包括：

- 检查本地是否存在指定的镜像，不存在就从公有仓库下载
- 利用镜像创建并启动一个容器
- 分配一个文件系统，并在只读的镜像层外面挂载一层可读写层
- 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
- 从地址池配置一个 ip 地址给容器
- 执行用户指定的应用程序
- 执行完毕后容器被终止

**关键点：**

- 容器的核心为所执行的应用程序，所需的资源都是应用程序所必须的
- 容器是否会长久运行，是和 docker run 指定的命令有关，和 -d 参数无关
- 当 Docker 容器中指定的应用终结时，容器也自动终止

**进入容器:**

```
docker run -dit ubuntu
docker container ls

docker attach 1ac38db98x  --> exit
docker exec -it 1ac38db98x bash  --> exit
```

docker attach CONTAINER
- Attach local standard input, output, and error streams to a running container
- 从这个 stdin 中 exit，会导致容器的停止

docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
- Run a command in a running container
- 从这个 stdin 中 exit，不会导致容器的停止


### docker volume --help



# Docker Compose
