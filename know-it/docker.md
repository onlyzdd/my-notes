# Docker 容器技术

Docker 是一种开源的 Linux 容器管理解决方案，通过进程和进程通信技术对操作系统的文件资源和网络进行隔离。每一个容器都有一个主进程，当该进程结束的时候，容器也会完全的停止。

相比于传统的虚拟机技术，Docker 的容器创建和管理开销更小，将容器内的进程直接运行在宿主机上，而且可以屏蔽诸多操作系统差异，容易移植且占用空间小。

## 基本概念

- Daemon：守护进程，需通过 Docker 提供的 API 才能访问
- Registry：仓库，存储镜像的仓库以供使用
- Container：容器，指镜像的运行时，可以看作是面向对象中的对象实例，包括了文件资源和系统资源
- Image：镜像，指应用打包好之后的存储方式，可以看作是面向对象中的类，包含多个层
- Layer：层，Dockerfile 每一步构建都会生成层
- Dockerfile：用于构建镜像的配置文件

## Docker 网络

Docker 默认以 bridge 桥接方式与容器通信，启动 Docker 后，宿主机上会产生 _docker0_ 的一个虚拟网络接口。当容器启动时，也会生成名为 _veth****_ 的网络接口，并以 _docker0_ 作为默认网关，容器退出后，该网络接口也会被销毁。

## Docker 命令

### Docker 命令导图

![Docker 命令导图](./docker/docker-command-diagram.jpg)

### 常用 Docker 命令

```sh
$ # 镜像相关
$ docker images # 查看本机的所有镜像
$ docker images [imageId] # 查看镜像信息
$ docker search [imageId] # 搜索远程仓库中的镜像
$ docker pull [imageId] # 下载镜像到本地，可指定版本
$ docker rmi [imageId] # 删除本地的镜像，可删除多个
$ docker history [imageId] # 查看镜像的创建历史
$ docker save [imageId] -o image.tar # 将镜像打包成文件
$ docker load -i image.tar # 从指定文件中加载镜像
$ # 容器相关
$ docker ps # 查看正在运行的容器
$ docker ps -a # 查看所有容器
$ docker run [imageId] # 如果不存在，则自动下载该镜像并启动容器，使用 --name 指定名称
$ docker run -it centos /bin/bash # 从镜像启动容器，并进入交互式命令行
$ docker run -d -p 80:8080 [imageId] # 后台启动容器，并将主机的80端口映射到容器的8080端口
$ docker start/stop/restart/kill [containerId] # 启动/停止/重启/强制停止容器
$ docker pause/unpause [containerId] # 停止/取消停止单个或多个容器
$ docker top [containerId] # 查看容器中的进程信息
$ docker inspect [containerId/imageId] # 查看镜像/容器的元信息
$ docker create [imageId] --name [containerId] # 从镜像创建容器，但不运行
$ docker exec -it [containerId] /bin/sh # 在运行中的容器中执行一个命令
$ docker attach [containerId] # 进入容器正在执行的终端，不建议使用
$ docker stats # 监控容器的资源使用情况
$ docker rename [containerId] [newContainerId] # 重命名容器
$ docker update --help # 更新一个或多个容器配置
$ docker logs [containerId] # 查看容器的日志
$ docker port [containerId] # 列出容器的端口映射关系
$ docker export [containerId] -o container.tar # 将容器打包成一个文件
$ docker import container.tar [containerId] # 从指定文件中加载容器
$ docker cp containerId:path1 path2 # 从容器拷贝文件到宿主机
$ docker cp path2 containerId:path1 # 从宿主机拷贝文件到容器
```

## Dockerfile

Dockerfile 用于定制镜像，包含 Dockerfile 目录的所有的所有内容称为上下文，上下文通过 `docker build -t name .`命令开始逐层构造镜像。在编写 Dockerfile 时，应该使执行的指令尽量少。

运行与 `docker build` 期间的指令有：

- `FROM <image>`：为后面的指令提供基础镜像
- `WORKDIR /path/to/workdir`：配置工作目录
- `COPY <src> <dest>`：复制文件或目录到新镜像中
- `ADD <src> <dest>`：与 `COPY` 类似，但可以指向网络文件
- `RUN ["command", "param1", "param2"]`：在前一条命令创建的基础上构建一个容器，在容器中运行命令，结束后 `commit` 容器为新镜像

运行于 `docker run` 期间的指令有：

- `CMD ["command", "param1", "param2"]`：在容器启动时执行的命令
- `ENTRYPOINT ["command", "param1", "param2"]`：与 `CMD` 命令类似
- `ENV <key> <value>`：为镜像创建的容器声明环境变量
