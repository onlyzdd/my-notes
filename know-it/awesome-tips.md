## 有用的技巧

### 利用 SOCKS5 代理提速 Github

- HTTP/HTTPS 提速

```bash
$ git config --global http.github.com.proxy 'socks5://127.0.0.1:1086' 
$ git config --global https.github.com.proxy 'socks5://127.0.0.1:1086'
```

- SSH 提速

```.ssh/config
Host github.com
    Hostname github.com
    ProxyCommand nc -x 127.0.0.1:1086 %h %p
```

- SSH 本地端口转发

```bash
$ ssh -L 9999:localhost:9999 nonroot@somedomain.com
```

- 利用 SSH 远程端口转发实现内网穿透

```bash
$ ssh -R subdomain:80:localhost:8080 serveo.net
```


