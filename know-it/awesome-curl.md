# 好用的 cURL 命令

## 获取网络上的资源

```sh
$ curl [URL] # 获取网络上的资源，这里将打印网页源代码
```

## 下载文件

```sh
$ curl [URL] -o index.html # -o 指定保存的文件名，-0 指定使用 http1.0，也可以指定 -O 使用默认文件名
```

## 跟踪重定向

```sh
$ curl -L [URL] # 自动跟踪网站重定向
```

## 显示响应头信息

```sh
$ curl -i [URL] # -i 指定显示响应头，-I 则只显示响应头
```

## 显示通信过程

```sh
$ curl -v [URL] # -v 指定显示通信过程的所有信息，常用于调试，可以使用 --trace 显示更加详细的过程
```

## 发送 HTTP 请求

```sh
$ curl [URL] # 默认发送 GET 请求
$ curl -X POST --data "username=curl" [URL] # -X 指定发送请求的方法（支持 PUT 和 DELETE），--data 指定发送的数据
```

## 提交 Form 表单

```sh
$ curl --form filename=@localfilename --form filed=value [URL] # --form 指定表单元素
```

## 携带 Cookie

```sh
$ curl --cookie "username=curl" [URL] # 发送 Cookie 并访问
```

## 修改请求头

```sh
$ curl --referer https://www.baidu.com [URL] # 修改 referer 字段并访问
$ curl --user-agent "[User Agent]" [URL] # 修改 User-Agent 字段并访问
$ curl --header "Content-Type: application/json" [URL] # 修改请求头并访问
```

## 使用代理

```sh
$ curl --proxy socks5://127.0.0.1:1086 [URL] # 以代理方式访问
$ curl --noproxy "*" [URL] # 对于指定的域名，不以代理访问
```


