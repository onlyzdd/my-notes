# Linux 快速上手

本文档旨在帮助快速熟悉 Linux 远程操作，全部内容在 Centos 7 下的 GNU Bash 4.2.46(2)-release (x86_64-redhat-linux-gnu) 测试通过。

## 准备工作

### SSH 远程登录

SSH 是专为远程登录提供安全性的协议，其具体实现有很多，这里以开源的 OpenSSH 为例。这也是 Linux 上最常用的版本，Windows 上使用可能略有不同。

在远程登录之前，你需要保证你在服务器上有自己的账号及密码（找系统管理员开账户）。以用户名 `user`，登录远程主机 `host` 为例，本地执行一条命令就可以登录到远程服务器上。

```bash
$ ssh user@host # user 是你的用户名，host 是远程主机地址，如 IP 和域名
$ ssh user@host -p 22222 # SSH 默认端口号是 22，如果服务器不是这个端口，可以通过 -p 修改
```

第一次登录时，需输入 `yes` 以确定连接。事实上，SSH 除了登录之外，还可以执行远程命令。

### 数据传输

通常，我们需要在服务器和本地之间进行数据传输，例如大文件的拷贝等。

一般而言，`scp` 命令可以做到主机间的文件拷贝，基本格式为。

```bash
$ scp user@host:/path/to/file /localpath/to/file # 将远程文件拷贝至本地
$ scp -r /localpath/to/folder user@host:/path/to/folder # 将本地目录及其下的所有文件拷贝到远程
```

另外，也可以使用 FileZilla 等基于 SFTP 协议的传输软件来进行数据传输，以及更加强大的 `rsync` 命令来进行数据同步。

### nohup 执行命令

有时我们需要执行非常耗时的命令，而当我们关闭终端时，远程命令就无法继续执行了。此时就需要用到 `nohup` 命令了，`nohup` 命令可以在关闭终端之后，使命令继续执行而不被停止。

```bash
$ nohup cmd & # 将命令执行放至后台
$ exit # 在不使用此终端时，正常退出，防止异常退出导致 nohup 命令失效
```

除了 `nohup` 命令，还可以使用终端多路复用软件，例如 `screen` 以及更好用的 `tmux`。

## 文件操作

Linux 上一切皆文件，目录也是文件的一种。

### 绝对路径和相对路径

Linux 上的路径包括绝对路径和相对路径，其中绝对路径是从根目录 `/` 开始的全路径，而相对路径则是相对于当前工作目录的路径。

```bash
$ pwd # 查看当前路径，即 print working directory，将返回以 / 开头的绝对路径
$ cd .. # 进入相对于当前目录的上一级目录
$ cd /root # 进入绝对路径 /root 中
```

### 文件的基本操作

- 创建文件

    `touch` 命令用来新建一个文件或修改文件的时间戳。

    ```bash
    $ touch 1.txt # 若文件不存在，则新建此文件，否则修改此文件的创建时间
    ```

- 移动或重命名文件

    `mv` 命令用来对文件进行移动或重命名，表示 Move。
    
    ```bash
    $ mv test.txt test.log # 将 test.txt 重命名为 test.log
    $ mv test.log logs # 将 test.log 移动到 logs 目录中，logs 目录必须事先存在
    $ mv -t ~/logs 1.log 2.log # 使用 -t 指定移动的目标目录，用来移动多个文件
    $ mv -i 1.txt 2.txt # 如果 2.txt 存在，询问是否覆盖
    $ mv -f 1.txt 2.txt # 直接用 1.txt 覆盖 2.txt
    ```

- 文件复制

    `cp` 命令用来复制文件，意为 Copy。

    ```bash
    $ cp 1.txt 2.txt # 复制 1.txt 到 2.txt
    $ cp 1.txt test # 复制 1.txt 到 test 目录下，使用 -i 进行交互式覆盖
    ```

- 删除文件

    `rm` 命令用于删除文件，是一个较为危险的命令，很多人在用此命令时通常会添加别名，并通过 `mv` 命令备份删除的文件，防止误删。

    ```bash
    $ rm a.txt # 删除 a.txt 文件，输入 y 删除，n 则不删除
    $ rm -f a.txt # 强制删除，不询问
    $ rm -i *.log # 交互式删除当前目录下的所有 .log 文件
    ```

### 目录的基本操作

- 列出目录

    `ls` 命令即 list，用来打印当前目录的清单。

    ```bash
    $ ls # 打印当前目录下的可见文件
    $ ls -l # 可简化为 ll 别名，打印所有可见文件的相关属性
    $ ls -a # 可简化为 la 别名，打印所有文件，包括不可见文件
    ```

- 进入目录

    `cd` 命令用来切换工作目录（change directory），也写作 `chdir`。

    ```bash
    $ cd / # 切换到系统根目录
    $ cd ~ # 切换到当前用户主目录，效果同 `~`、`cd`
    $ chdir .. # 切换到上一级目录
    $ cd - # 切换到进入当前目录之前所在的目录，并输出此目录的绝对路径，效果同 `-`
    ```

- 创建目录

    `mkdir` 命令用来创建目录（Make Directory），如果文件夹已存在，则不会创建成功。

    ```bash
    $ mkdir a # 创建目录
    $ mkdir c d # 同时创建多个目录
    $ mkdir -p b/bb # 递归创建多个目录
    $ mkdir -p a # 如果目录 a 不存在，则创建目录 a
    $ mkdir -m 777 e # 创建权限为 777 的目录 a
    $ mkdir -v f # 创建目录时输出信息
    $ mkdir -vp scf/{lib/,bin/,doc/{info,product},logs/{info,product},service/deploy/{info,product}} # 创建一个项目目录
    ```

- 移除目录

    可以使用 `rmdir` 命令来删除文件夹，但更常用的是使用 `rm` 命令，需添加 `-r` 参数才能删除一个非空文件夹。
    ```bash
    $ rm -r dir_a # 删除目录 dir_a，并递归删除其下的所有目录和文件
    ```

- 移动或重命名目录

    `mv` 命令还用来对目录进行移动或重命名，表示 Move。
    
    ```bash
    $ mv test1 test2 # 如果 test2 不存在，则认为是重命名，如果 test2 是目录，则将 test1 移动到该目录下
    ```

- 文件和目录复制

    `cp` 命令还可以用来复制目录，意为 Copy。

    ```bash
    $ cp -a test1 test2 # 如果 test2 目录不存在，则拷贝 test1 并命名为 test2，如果 test2 存在，则将 test1 拷贝到该目录下，-a 参数表示拷贝 test1 下的所有文件和目录
    ```

### 文件和目录的权限

Linux 为每个文件都定义了文件所有者、所有组、其他人的权限，权限也包括读、写、执行三种。在 Linux 中，经常需要管理文件权限。

- 查看权限

    当使用 `ls -l` 命令时会列出文件的详细信息，输出有 7 列。第一列是文件类别和权限，这列由 10 个字符组成。第一个字符表示该文件的类型，`d` 表示文件夹，`-` 表示普通文件。第 2~4 个字符表示给文件所有者的权限，第 5~7 个字符表示给文件所有组的权限，第 8~10 个字符表示给其他用户的权限。`r` 表示读权限，`w` 表示写权限，`x` 表示执行权限，如果不具备权限则显示 `-`。第二列表示文件和目录的连接数，第三列为文件的所有人，第四列为文件的所有组，第五列为该文件的大小，第六列是该文件的最近修改时间，第七列是文件名。

    ```bash
    $ ll a.txt => -rwxrwxr-x 1 root root 180 Oct  1 16:44 a.txt
    ```

- 修改权限

    `chmod` 命令用来改变文件或目录的访问权限，该命令支持包含字母和操作表达式的文字设定法、包含数字的数字设定法。

    ```bash
    $ chmod -R a+r a # 增加目录所有用户组的读权限，-R 递归目录下的所有文件和目录
    $ chmod 777 a.sh # 最大化所有权限，r 为 4，w 为 2，x 为 1，效果同 chmod u=rwx a.sh
    ```
- 改变文件的所有者

    `chown` 命令将指定文件的拥有者为指定的用户或组（Change Owner），经常为系统管理员使用。

    ```bash
    $ chown :test a.txt # 改变 a.txt 的所有组为 test
    $ chown -R test a # 改变 a 文件夹及其下的所有递归文件和子目录的所有者为 test
    $ chown test:test a.txt # 同时改变 a.txt 的所有者和所有组
    ```

- 改变文件的拥有组

    `chgrp` 命令用来变更文件和目录所属群组（Change Group）。

    ```bash
    $ chgrp -R staff mydir # 改变文件夹 mydir 及其所有子文件、子文件夹的所属组为 staff
    ```

### 查找文件

- 一般查找

    `find` 命令沿着文件层次结构向下遍历，匹配符合条件的文件，执行相应的操作。

    ```bash
    $ find PATH -name FILENAME # 寻找 PATH 目录下名为 FILENAME 的文件，并输出其路径
    $ find /etc -name *.conf # 使用通配符来匹配文件，使用时注意权限问题
    ```

    常见的参数还有：

    | 参数 | 作用 |
    | --- | --- |
    | -name filename | 指定要查找的文件名 |
    | -user username | 指定要查找文件的所有者 |
    | -perm ddd | 指定要查找文件的权限 |
    | -maxdepth n | 指定查找深度 |
    | -delete | 删除匹配到的文件 |
    | -exec cmd {} | 用于执行命令，{} 是匹配到的文件名 | 

- 查找执行文件

    `which` 命令是根据 `PATH` 环境变量定义的目录，来定位文件绝对路径的命令。`whereis` 功能与 `which` 类似，不过它还能找到相关的 `man` 文件。

    ```bash
    $ which cd # => /usr/bin/cd
    $ whereis cd # => cd: /usr/bin/cd /usr/share/man/man1/cd.1.gz /usr/share/man/man1p/cd.1p.gz /usr/share/man/mann/cd.n.gz
    ```

### 文件归档、压缩

- 文件归档

    `tar` 命令可以对文件进行归档和从归档中提取文件。

    ```bash
    $ tar -cf output.tar file1 folder2 # -cf 创建归档
    $ tar -tvf output.tar # -tf 列出归档，-v 表示打印输出
    $ tar -xvf output.tar -C /path/to/outdir # -xf 从归档中提取文件，-C 指定提取目录
    $ tar -f output.tar --delete file1 # 从归档中删除指定文件
    $ 
    ```

- 压缩和解压缩

    `gzip` 是 Linux 系统中常用的对文件进行压缩和解压缩的命令，通常和 `tar` 命令结合使用，对文本文件有 60%~70% 的压缩率。

    ```bash
    $ tar -acvf output.tar.gz file1 file2 folder1 # 根据扩展名自动压缩归档，其他的压缩格式选项有 -j、-z、--lzma
    $ tar -axvf input.tar.gz # 根据扩展名自动解压缩
    $ gzip -l * # 显示每个压缩的文件信息，并不解压
    ```

### 内存和存储

- 内存占用

    `free` 命令可以显示系统中空闲内存、已用的物理内存以及交换内存，是常用的监控工具（MacOS 上不存在）。

    ```bash
    $ free # 显示内存使用情况，通常使用 -mh 参数对输出单位格式化
    ```

- 文件系统的磁盘占用

    `df` 命令用来检查文件系统的磁盘空间占用情况。

    ```bash
    $ df # 显示磁盘使用情况，-h 参数使输出更易读
    ```

- 文件和目录的磁盘占用

    `du` 命令与 `df` 命令不同，其用来查看文件和目录的磁盘占用空间。

    ```bash
    $ du folder # 递归显示目录和文件所占空间
    $ du -c a.txt b.txt # 查看 a.txt、b.txt 大小并统计总和
    ```

- 挂载和卸载磁盘

    在挂载磁盘之前，磁盘必须已经连接到主机上，可以通过 `fdisk -l` 命令或 `lsblk` 命令来查看磁盘挂载情况。

    ```bash
    $ fdisk -l # 输出磁盘挂载情况，也可以使用 lsblk
    ```

    在上面命令的输出中找到要挂载的磁盘，通过 `mount` 命令进行挂载，之后就可以通过挂载点访问磁盘上的文件系统了。

    ```bash
    $ mount DEVICE_PATH MOUNT_POINT # DEVICE_PATH 指具体的设备，即 fdisk -l 的输出，MOUNT_POINT 是挂载点，需要保证挂载点是一个新建的目录
    $ cd MOUNT_POINT && ls # 操作磁盘
    $ mount # 显示所有挂载
    ```

    当不需要使用外置磁盘，也就是要卸载时，使用 `umount` 命令解除挂载。

    ```bash
    $ umount DEVICE_PATH # 效果同 umount MOUNT_POINT
    ```

## 进程管理

进程是应用程序的运行实例，是操作系统进行资源调度和分配的一个基本单位。

### 查看

- top

    `top` 命令提供了实时的系统状态监控，可以按照 CPU 占用、内存占用、执行时间等指标对进程进行排序。

    ```bash
    $ top # 显示实时的系统状态，q 键退出
    ```

    其输出的面板中，上部分是系统的基本信息，下部分的表格是进程的动态信息。字段说明如下：

    | 字段 | 含义 |
    | --- | --- |
    | PID | 进程唯一的 ID |
    | USER | 进程所有者 |
    | VIRT | 进程使用的虚拟内存总量，VIRT=RES+SWAP |
    | RES | 进程使用的实际物理内存大小，RES=CODE+DATA |
    | %CPU | CPU 时间占用百分比 |
    | %MEM | 物理内存占用百分比 |
    | TIME+ | 进程使用的 CPU 时间总计 |
    | COMMAND | 进程名称（命令名称） |

- ps

    `ps` 命令是一个非常强大的进程查看工具，其也有非常多的参数。

    ```bash
    $ ps -A # 列出所有的进程，和 -e 有一样的效果
    $ ps -u root # 查看 root 用户的所有进程
    $ ps -ef # 显示所有进程信息，连同执行的命令
    $ ps -l # 显示本次登录后的执行的进程
    $ ps aux # 列出目前所有在内存中的程序，-w 可以进行显示加宽
    ```

### 终止

- kill

    要终止一个进程，通常使用 `kill` 命令来实现。`kill` 命令向内核发送一个系统信号以及进程的 PID，使得内存对该进程进行某些操作。

    一般来说，`kill` 命令经常要和 `ps` 命令一起使用，因为 `kill` 命令需要有进程 PID。

    ```bash
    $ kill -9 PID # 杀死指定 PID 的进程
    ```

- killall

    由于 `kill` 命令在使用时需要先查询到要终止的进程的 PID，使用起来相对麻烦。`killall` 命令可以解决这个问题，`killall` 根据进程的名称而不是进程 PID 来结束进程。

    ```bash
    $ killall httpd # 杀死所有 httpd 进程
    ```

### 强大的 lsof

`lsof` 用于列出当前系统中所有打开文件，如果此命令不存在，则可以通过 `yum install lsof -y` 安装。由于 Linux 中一切皆文件，所以此命令功能极为强大。

```bash
$ lsof filename # 显示打开指定文件的进程
$ lsof -c string # 显示 COMMAND 列包含指定字符串进程所打开的所有文件
$ lsof -u username # 显示 username 用户打开的文件
$ lsof +d dir # 显示目录下被进程打开的文件，+D 将递归搜索目录
$ lsof -i [protocol][@hostname|hostaddr][:service|port] # 显示符合条件的进程情况，此方法可以用来查看端口占用，例如 lsof -i :8080
```

## 软件安装

### 源代码编译安装

由于计算机无法运行高级语言编写的源程序，所以很多程序在运行前，都需要对其进行编译，以产生可执行文件。通常，为了方便软件安装，源代码的构建都会编写在 _Makefile_ 文件中，一般来说执行以下命令即可安装（_Makefile_决定安装方式）。

```bash
$ ./configure # 生成必要参数和 Makefile 文件
$ make # 执行编译
$ make install # 执行安装
```

### RPM 安装

RPM 包管理提供了较为简单的软件安装方式，只需下载软件包再执行安装命令即可，常见使用如下：

```bash
$ rpm -ivh PACKAGE_NAME-VERSION.rpm # 安装软件包
$ rpm -Uvh PACKAGE_NAME-VERSION.rpm # 更新软件包
$ rpm -q PACKAGE_NAME # 查看软件是否已安装
$ rpm -e PACKAGE_NAME # 卸载软件包
```

### yum 安装

`yum` 是基于 RMP 的 shell 前端包管理器，使用起来比 RPM 更加方便，并且可以自动解决依赖关系。

```bash
$ yum install PACKAGE # 安装软件包
$ yum remove PACKAGE # 卸载软件包
```

## 文本处理

### 查看文件

- cat 命令

    `cat` 命令常用于读取、显示或拼接文件内容，也叫做拼接（concatenate）命令。

    ```bash
    $ cat file1 file2 file3 # 显示多个文件拼接后的内容
    $ echo "Text through stdin" | cat - file.txt # 使用管道符从标准输入中读取，并将其与 file.txt 进行合并，- 被作为 stdin 文本的文件名
    $ cat -s file.txt # 删除输出中多余的空白行，连续的空白行只显示一个
    $ cat -n file.txt # 在每一行输出前加上行号，可以通过 -b 选项取消对空白行的编号
    ```

- less/more

    `more` 命令功能类似于 `cat`，不过 `cat` 会将整个文件内容从上到下显示在屏幕上。`more` 命令在启动时就加载整个文件，并会一页一页显示以方便阅读，使用空格键显示下一页，使用 _b_ 显示上一页，而且支持字符串搜索。

    ```bash
    $ more +3 1.txt # 显示文件中从第 3 行起的内容
    $ more +/text 1.txt # 从文件中查找第一个出现 text 字符串的行，并从该处前两行开始显示输出
    $ ls -l | more -5 # 设定每屏显示的行数为 5 来显示当前目录下的所有文件
    ```

    `less` 命令是对文件或其他输出进行分页显示的工具，相比于 `more` 功能更强。

    ```bash
    $ less a.txt # 分页查看文件，使用回车键到下一行，空格键到下一页
    $ history | less # 分页显示命令历史记录，输入 `/str` 进行向下搜索，使用 `?str` 进行向上搜索，-i 参数忽略搜索大小写，键盘 n 和 N 表示重复上一个搜索和反向重复上一个搜索
    $ less a.txt b.txt # 浏览多个文件，输入 n 切换到下一个文件，输入 p 切换到上一个文件
    ```

- head/tail

    `head` 命令就像它的名字一样，用来显示档案的开头。

    ```bash
    $ head a.txt # 显示 a.txt 的前 10 行
    $ head -n 5 a.txt # -n 指定显示的行数，在 Linux 行数为负数表示除最后几行的所有行
    $ head -c 30 a.txt # -c 指定显示的字节数
    ```

    `tail` 命令就像它的名字一样，用来显示档案的结尾。

    ```bash
    $ tail a.txt # 显示 a.txt 的最后 10 行
    $ tail -n 5 a.txt # -n 指定显示的行数
    $ tail -c 30 a.txt # -c 指定显示的字节数
    $ tail -f a.log # -f 循环查看文件内容，当文件在改变时，此命令很有用
    $ tail -n +5 a.txt # 从第 5 行显示文件
    ```

### 字符处理

- I/O 重定向

    I/O 重定向可以将任何文件、命令、脚本等的输出重定向到另外一个文件、命令、脚本中。I/O 重定向常见符号和功能描述如下：

    | 符号 | 含义 |
    | --- | --- |
    | > | 标准输出覆盖重定向：将输出重定向到其他文件中，会覆盖原有内容 |
    | >> | 标准输出追加重定向：将输出追加到其他文件中 |
    | >& | 标识输出重定向：将一个标示的输出重定向到另一个标示的输出中 |
    | < | 标准输入重定向：命令从指定文件中读取输入 |
    | \| | 管道：从一个命令中读取输出作为下一个命令的输入，使用非常频繁 |

    ```bash
    $ nohup bash test.sh 2>&1 & # nohup 执行 bash test.sh 并将错误输出重定向到标准输出
    $ history | less # 分页查看历史记录
    ```

- grep

    `grep` 命令是非常强大的文本搜索工具，如果匹配到相关信息，就打印出符合条件的行，对正则有简单的支持。

    ```bash
    $ grep [-ivnc] string file # 例如 grep -c 'name' a.log，
    # -i: 不区分大小写
    # -v: 反向匹配，即不包含指定模式
    # -n: 同时输出匹配的行号
    # -c: 只输出包含匹配的行数
    $ cat a.txt | grep $'^1\t' # 使用 C 模式匹配，这种写法更加强大
    ```
- sort

    `sort` 将文件的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按 ASCII 码值进行比较，最后将它们按升序输出。

    ```bash
    $ sort -[ntkr] file
    # -n: 采用数字排序
    # -t: 指定分隔符，默认为单个空格、制表符
    # -k: 指定要排序的列
    # -r: 进行反向排序
    ```

- uniq

    `uniq` 命令用于对存在多行相同内容的文本进行去重，此命令只处理相邻行的重复问题，所以一般要与 `sort` 命令一起使用。`-i` 参数忽略大小写，`-c` 参数输出重复的行数。

    ```bash
    $ cat a.txt | sort | uniq # 对 a.txt 相同行进行去重
    ```
- cut

    `cut` 是截取的意思，用来取出行中的指定部分。

    ```bash
    $ cat /etc/passwd | cut -f 1,6 -d ':' # 指定冒号为分隔符，指定列为 1、6 两列
    ```
- tr

    `tr` 命令用于文本转换或删除。
    ```bash
    $ cat /etc/passwd | tr '[a-z]' '[A-Z]' # 将小写字母转换为大写字母
    $ cat /etc/passwd | tr -d ':' # 删除文本中的冒号
    ```

- paste

    `paste` 命令用来将文件按照行进行合并，默认中间用制表符隔开，可以使用 `-d` 参数指定分隔符。

    ```bash
    $ paste -d ":" a.txt b.txt # paste 按行对文件进行合并，分隔符为冒号
    ```

- split

    `split` 命令用来将文件分割成多个更小的片段，是分批处理文件的很好方案。

    ```bash
    $ split -b 10k data.file -d -a 4 split_file # 把 data.file 文件以 10KB 大小分割，d 选项表示使用数字为后缀，a 选项表示后缀长度，split_file 为文件名前缀
    $ split -l 500 data.file # 根据行数分割文件
    ```

### vim 编辑

`vim` 编辑器是 `vi` 编辑器的增强版，方便编程编辑，是一个效率很高的编辑器。但不能使用 `vim` 处理非常大的文本文件，需要处理大文件时，需要使用 `sed` 等。

- vim 的三种模式

    `vim` 包括一般模式、编辑模式、末行指令模式这 3 种模式。
    - 一般模式： `vim` 打开文件后默认进入的模式，此模式下可以移动光标，还可以使用组合键执行编辑命令。
    - 编辑模式：一般模式下，按 _i_ 键进入编辑模式，此时可以通过键盘输入文字。按 _Esc_ 键退回到一半模式。
    - 末行指令模式：一般模式下，按 _:_、_/_ 或 _?_ 键进入末行指令模式，输入指令完成操作

- vim 效率提升

编辑 `~/.vimrc` 文件以增强 `vim` 的使用效率。

```.vimrc
set nu!
syntax on
set tabstop=4
```

### sed 文本处理

`sed` 是一种非交互的流编辑器（Stream Editor），`sed` 不会直接修改源文件，而是将处理的结果打印到标准输出。`sed` 处理文本是以行为单位的，每处理完一行就打印出来，然后处理下一行直到全文处理完成。

`sed` 能够处理大型文件，并加快有规律文件的修改，`sed` 可以完成删除、查找替换、添加、插入等动作，而且还支持正则表达式以及 Sed 脚本。

```bash
$ sed '1d' test.txt # 删除第一行后输出结果，不改变源文件
$ sed -i '1,3d' test.txt # 删除指定范围的行，-i 直接修改源文件
$ sed 's/line/LINE/2' test.txt # 替换前 2 个 line 为 LINE，默认值是 1 不用写，g 表示替换所有
$ sed 'y/1234/ABCD/' test.txt # 字符转换
$ sed -e '2 i Insert' -e '2 a Insert' test.txt # -e 连接多个命令，i、a 分别表示在行前、后插入行
$ sed -n '1p' test.txt # p 表示打印，-n 不打印无关的行
```

### awk 文本处理

`awk` 同样以行为单位读入文本，并将行按字段切分（默认分隔符为连续的空格、制表符），方便处理格式化的文件。`awk` 本身也是一种编程语言，这也使得 `awk` 处理格式化的文本更加灵活而强大。同时，`awk` 还支持读入脚本。

```bash
$ awk '{print $1, $3}' test.txt # 输出第一列和第三列，$0 表示此行
$ awk -F '.' '{print $1, $2}' test.txt # 指定分隔符
$ awk '{print NF}' test.txt # 内部变量 NF 表示行中字段个数
$ awk '{print $(NF-1)}' test.txt # 打印倒数第二列
```

## 实用技巧

### 命令序列

命令序列允许我们在不同情况下执行命令。

```bash
$ cmd           # 最常见的执行方式
$ cmd1 ; cmd2   # 不管前面的命令是否执行成功，后面的命令都会继续执行
$ cmd1 & cmd2   # 命令同时执行
$ cmd1 && cmd2  # 前面的命令执行成功后才会执行后面的命令
$ cmd1 || cmd2  # 前面的命令执行失败后才会执行后面的命令
```

### 查看和编辑大文件

- 查看大文件：请一定不要使用 `cat` 等命令，请务必使用 `less` 命令，而且在于查找等方面也更加方便。
- 编辑大文件：请一定不要使用 `vim` 等命令，请务必使用 `sed` 或 `awk` 命令，`vim` 读入并显示大文件会直接崩溃。

### 查看动态变化的日志文件

程序运行时可能会持续地向日志文件中写入记录，要想动态地查看这种日志文件，可以使用：

```bash
$ tail -f nohup.out # 持续查看 nohup.out 文件，以跟踪变化
```

### 查看文件和目录类型

`file` 命令可以用来查看文件和目录的文件类型，在查看无扩展名文件或以 `.bin` 结尾的文件会很有用。

```bash
$ file /etc/passwd # => /etc/passwd: ASCII text
$ touch a && file a && rm a # => a: empty
$ file test.pdf # => test.pdf: PDF document, version 1.3
```

### 命令未找到

Bash 中执行命令时，会从 `PATH` 环境变量中寻找该命令（还有其他的方式，比如别名等）。如果提示命令未找到，一般情况下都是 `PATH` 环境变量的问题，可以通过 `echo $PATH` 查看环境变量是否正确。解决方式一般是将可执行文件的路径添加到 `PATH` 中，另一种是通过 `ln -s` 直接为该命令创建软链接，保证软链接所在位置在 `PATH` 中就好。

## 附录

### Bash 快捷键

以下为 Mac 版本的设置，Windows、Linux 上的 Alt 键就是 Option 键。对于 Mac，需要设置 Option 键为 Meta 键，具体方式：终端 - 偏好设置 - 键盘 - 将 Option 键作为 Meta 键。

- Ctrl + A：移到命令行首
- Ctrl + E：移到命令行尾
- Option + F：按单词向前移动（Forward），也可以使用左方向键
- Option + B：按单词向后移动（Back），也可以使用右方向键
- Ctrl + U：从光标处删除至命令行首
- Ctrl + K：从光标处删除至命令行尾
- Option + Backspace：从光标处删除至单词首
- Option + D：从光标处删除至单词尾
- Option + .：使用上一条命令的最后一个参数
- Ctrl + R：逆向搜索命令历史，使用 Ctrl + G 退出搜索
- Ctrl + L：清屏
- Ctrl + C：终止命令
- Ctrl + Z：挂起命令，使当前终端不被之前的命令占用，并将该进程放至后台执行

### 参考链接

- VSCode
    - [VSCode 远程开发](https://code.visualstudio.com/docs/remote/ssh-tutorial)
- Tmux
    - [Tmux 终端多路复用](https://github.com/tmux/tmux/wiki)
- Zsh
    - [Zsh: 更强的 Shell](http://www.zsh.org/)
    - [oh-my-zsh：增强 Zsh](https://ohmyz.sh/)
- Vim
    - [Vim 快捷键](http://cenalulu.github.io/linux/all-vim-cheatsheat/)
- Sed
    - [Linux 命令大全：Sed 命令](http://man.linuxde.net/sed)
- Awk
    - [极客学院：Awk 命令](http://wiki.jikexueyuan.com/project/awk/overview.html)
    - [Awk 程序设计语言](https://github.com/wuzhouhui/awk/raw/master/The_AWK_Programming_Language_zh_CN.pdf)
- Shell 脚本编程
    - [10 分钟入门 Shell 脚本编程](https://juejin.im/post/5a6378055188253dc332130a)
    - [Shell 脚本变量判断](https://renwole.com/archives/1204)
