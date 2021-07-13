# 上手 Nginx

## Nginx 简介

Nginx（Engine X）是一款开源的、高性能的、轻量级 HTTP 服务器和反向代理服务器，使用基于事件驱动架构，使得其可以支持数百万级别的 TCP 连接。

### Nginx 安装

Nginx 是一个跨平台的服务器，在多数平台上是有发行版本的。此外，Nginx 还支持从源码构建可执行文件。然而，由于 Nginx 是高度模块化的，且编译时未包含默认模块，因此，自行编译时需要手动指定需要安装的模块。

### Nginx 命令行参数

- `-? | -h`：输出命令帮助页面
- `-c file`；手动指定配置文件，而不使用默认值
- `-e file`：手动指定错误日志文件路径，而不使用默认值
- `-g directives`：设置全局配置指令
- `-p prefix`：设置 nginx 路径前缀，即默认存储静态文件的目录
- `-q`：测试配置文件时，忽略除错误外的其他输出
- `-s signal`：向主进程发送信号 `stop`、`quit`、`reload`、`reopen`
- `-t`：测试配置文件，包括语法检测及文件打开
- `-T`：与 `-t` 类似，不过会打测试的印配置文件
- `-v`：打印 Nginx 版本号
- `-V`：打印 Nginx 版本号、编译器版本以及配置参数

### Nginx 配置文件

Nginx 及模块行为由配置文件中的指令决定，指令包括简单指令和块级指令。

- 简单指令：单行，名称和参数中间用空格隔开，必须用分号结尾，如 `root`、`listen` 等
- 块级指令：由花括号包围，与内部指令形成上下文，如 `server`、`http` 等


### Nginx 小贴士：

- 使用 `nginx` 命令启动 Nginx
- 可以通过 `nginx -V` 命令查看默认的配置文件路径
- `nginx -s signal` 尽量使用 `quit` 而不是 `stop`
- 在执行 `nginx -s reload` 之前，可以使用 `nginx -t` 测试配置文件
- Nginx 运行时，包含一个 main 进程及一个或多个 worker 进程
- 必须要熟悉 `nginx.conf` 中的配置，尤其是 `http` 下的 `server`
- 在配置文件中使用 `#` 提供注释

## Nginx 的使用场景

### HTTP 服务器

Nginx 可以作为一个 HTTP 服务器，托管静态 Web 资源。例如：

```
server {
    listen 80;
    location / {
        root /usr/share/nginx/html; # 静态文件路径
    }
}
```

### 反向代理

Nginx 可以作为反向代理，将客户端请求转发应用服务器，然后将结果返回客户端。例如：

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:8080; # 应用服务器地址
    }
}
```

### 负载均衡

Nginx 可以作为负载均衡服务器，将大量的客户端请求分配给多台应用服务器。例如：

```
upstream myapp {
    server 192.168.1.123 weight=3;
    server 192.168.1.124;
    server 192.168.1.125 down;
}
server {
    listen 80;
    location / {
        proxy_pass http://myapp;
    }
}
```

### 虚拟主机

Nginx 可以作为虚拟主机（Web 概念），通过 `server_name` 和 `listen` 来实现将不同的网站部署到同一台服务器上。例如；

```
server {
    listen 80;
    server_name aaa.com;
    location / {
        proxy_pass http://localhost:8080;
    }
}
server {
    listen 80;
    server_name bbb.com;
    location / {
        proxy_pass http://localhost:8081;
    }
}
```

## 参考资料

- [Nginx 快速入门](https://www.nginx.com/resources/wiki/start/)
- [Nginx 官方文档](https://nginx.org/en/docs/)
- [acme.sh 配置 HTTPS](https://github.com/acmesh-official/acme.sh)
