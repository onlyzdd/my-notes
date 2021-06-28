## 有用的技巧

### Linux

- Bash 内置命令中的双破折号 `--`
- 查看归档或压缩包中的文件

### SOCKS 代理

- HTTP/HTTPS 提速 Github

```bash
$ git config --global http.github.com.proxy 'socks5://127.0.0.1:7890'
$ git config --global https.github.com.proxy 'socks5://127.0.0.1:7890'
```

- SSH 提速 Github

```.ssh/config
Host github.com
    Hostname github.com
    ProxyCommand nc -x 127.0.0.1:7890 %h %p
```

- SSH 本地端口转发

```bash
$ ssh -L 9999:localhost:9999 nonroot@somedomain.com
```

- 利用 SSH 远程端口转发实现内网穿透

```bash
$ ssh -R subdomain:80:localhost:8080 serveo.net
```
