# SSH基本原理及应用

## SSH是什么？

SSH（Security Shell）是建立在应用层基础上的安全协议，专为远程登录会话和其他网络服务提供安全性的协议。

需要明确的是，SSH仅仅是一种协议标准，其具体实现很多，比较有名而且使用广泛的是开源的[OpenSSH](http://www.openssh.com/)。本文的实现是基于OpenSSH的，各种实现之间可能有差异。

SSH区别于其他协议主要体现在安全性，其主要用途包括远程登录和端口转发。

## 原理

SSH的基本原理是：（1）远程主机收到用户的登录请求，把自己的公钥发给用户。（2）用户使用这个公钥，将登录密码加密后，发送回来。（3）远程主机用自己的私钥，解密登录密码，如果密码正确，就同意用户登录。

由于SSH协议的公钥是自己签发的，没有证书中心（CA）认证的，所以很难处理[中间人攻击](http://en.wikipedia.org/wiki/Man-in-the-middle_attack)。

## 远程登录

远程登录是SSH最主要的用途，可以分为口令登录和公钥登录。以用户名`user`，登录远程主机`host`，只需要：

```bash
$ ssh user@host
```

SSH默认的端口号是`22`，使用`-p`参数，可以修改这个端口：

```bash
$ ssh user@host -p 43220
``` 

### 口令登录

口令登录就是使用用户密码进行登录，在第一次登录远程主机时，无法确认远程主机的真实性，只知道其发过来的公钥指纹（对公钥加密后的MD5），需要用户自行确认是否继续连接。

```
$ ssh user@host
The authenticity of host 'host (d.d.d.d)' can't be established.

RSA key fingerprint is ad:ad:ad:ad:ad:ad:ad:ad:ad:ad:ad:ad:ad:ad:ad:ad.

Are you sure you want to continue connecting (yes/no)?
```

一般情况下，这个时候都要输入`yes`，才能连接上。之后，会提示远程主机得到认可，并要求输入密码：

```
Warning: Permanently added 'host,d.d.d.d' (RSA) to the list of known hosts.
Password: (enter password)
```

当远程主机得到认可后，会将其信息保存在本地主机文件`$HOME/.ssh/known_hosts`中，下次连接时就不会警告。

### 公钥登录

使用口令登录每次都要输入密码很麻烦，SSH对此还提供了公钥登录，可以不用输入密码。

公钥登录，就是用户将自己的公钥储存在远程主机上。登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回来。远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录，不再要求密码。

这种方法要求用户提供自己本地主机的公钥，可以使用ssh-keygen生成（如果本地有千万就不要运行下面的命令了，除非在生成时修改保存位置，新手不要误操作）：

```bash
$ ssh-keygen
```

你还可以提供一些其他信息，例如备注和私钥口令。一般直接一路回车就好，最后，在`$HOME/.ssh`目录下会生成两个文件：`id_rsa.pub`和`id_rsa`，前者是公钥，后者是私钥。注意，我们要分发的是公钥，一定不能把私钥给分发了。

接着，把公钥发送到远程主机上。在使用下面这个命令之前，请确保远程主机中的`$HOME/.ssh/authorized_keys`文件不存在或为空，否则当前的命令会将原来文件中的公钥覆盖。

```bash
$ ssh-copy-id user@host
```

这样，在登录时，就不用输入密码了。

如果`$HOME/.ssh/authorized_keys`中已有其他人的公钥，就一定不能使用上面的命令了，我们需要将自己的公钥追加到这个文件的末尾。可以使用下面的命令。

```
$ ssh user@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
```

这条命令依次执行了：

- `"$ ssh user@host"`，表示登录远程主机；
- 单引号中的`mkdir .ssh && cat >> .ssh/authorized_keys`，表示登录后在远程shell上执行的命令：
- `"$ mkdir -p .ssh"`的作用是，如果用户主目录中的`.ssh`目录不存在，就创建一个；
- `'cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub`的作用是，将本地的公钥文件`~/.ssh/id_rsa.pub`，重定向追加到远程文件`authorized_keys`的末尾。

如果在执行此命令之后仍然无法免密登录，其原因可以通过 `ssh -v user@host` 进行调试查看。很多情况是远程文件的权限错误，可以尝试 `chmod 700 .ssh && chmod 600 .ssh/authorized_keys`。

> 小技巧

当我们通过SSH在远程主机上执行比较耗时的命令行任务时，无操作的情况下在程序运行结束前就会发现，我们连接的窗口失去了连接。事实上，这种网络超时的原理非常复杂，而且很多情况下服务器和本地都可以对此进行设置，所以没有万能的解决方法。一般地，可以配置本地主机文件`$HOME/.ssh/config`，以每隔一定的时间就向远程主机发包，以保持连接，在上面的文件中写入：

```
# send no-op package every 60s to keep connection alive
Host *
    ServerAliveInterval 60
```

每次在进行SSH连接时，都输入用户名、主机和端口好很麻烦，这时可以配置本地主机文件`$HOME/.ssh/config`（如果没有就创建），写入或追加：

```
Host baidu
    Hostname baidu.com
    Port 43220
    User liyanhong
```

在命令行输入`ssh baidu`，就相当于：

```bash
$ ssh liyanhong@baidu.com -p 43220
```

## 端口转发

有时因为网络配置问题，有些服务器是无法让外网用户直接访问的，这时可以使用端口转发的方法。例如当本地主机可以外网访问，而远程主机只能内网访问的情况下：

```bash
$ ssh -L 8080:localhost:8888 user@host
```

它表示将本地主机的`8080`端口绑定到`host`主机的`8888`端口，这样访问本地主机的`8080`端口就相当于访问远程主机的`8888`端口了，这种方式又被称为SSH隧道。
