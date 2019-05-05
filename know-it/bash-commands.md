# 常用的 Bash 命令

Bash 是许多 Linux 发行版中默认的 Shell，本文档介绍 Bash 中常用的命令，掌握这些命令在 Linux 的使用中十分重要。注意，本文档的绝大多数案例仅在 MacOS（GNU bash, version 3.2.57(1)-release (x86_64-apple-darwin17)，Copyright (C) 2007 Free Software Foundation, Inc.）中测试通过。

### `cal` 命令

`cal` 命令用来显示日历。

```bash
$ cal # 显示当前月的日历
$ cal 12 2012 # 查看 2012 年 9 月的日历
$ cal -j # 显示在当年中的第几天
$ cal -y 2013 # 显示当前年的日历，默认为当前年
```

### `cat` 命令

`cat` 命令常用于读取、显示或拼接文件内容，也叫做拼接（concatenate）命令。

```bash
$ cat file1 file2 file3 # 显示多个文件拼接后的内容
$ echo "Text through stdin" | cat - file.txt # 使用管道符从标准输入中读取，并将其与 file.txt 进行合并，- 被作为 stdin 文本的文件名
$ cat -s file.txt # 删除输出中多余的空白行，连续的空白行只显示一个
$ cat -n file.txt # 在每一行输出前加上行号，可以通过 -b 选项取消对空白行的编号
```

### `cd` 命令

`cd` 命令用来切换工作目录（change directory），也写作 `chdir`。

```bash
$ cd / # 切换到系统根目录
$ cd ~ # 切换到当前用户主目录，效果同 `~`、`cd`
$ chdir .. # 切换到上一级目录，可以连续使用
$ cd - # 切换到进入当前目录之前所在的目录，效果同 `-`
```

### `chgrp` 命令

`chgrp` 命令用来变更文件和目录所属群组（Change Group）。

```bash
$ chgrp -v staff a.txt # 改变文件 a.txt 的所属组为 staff
$ chgrp -R staff mydir # 改变文件夹 mydir 及其所有子文件、子文件夹的所属组为 staff
```

### `chmod` 命令

`chmod` 命令用来改变文件或目录的访问权限，该命令支持包含字母和操作表达式的文字设定法、包含数字的数字设定法。

```bash
$ chmod a+x a.sh # 增加文件所有用户组的可执行权限
$ chmod 777 a.sh # 最大化所有权限
```

### `chown` 命令

`chown` 命令将指定文件的拥有者为指定的用户或组（Change Owner），经常为系统管理员使用。

```bash
$ chown :test a.txt # 改变 a.txt 的所有组为 test
$ chown -R test a # 改变 a 文件夹及其下的所有递归文件和子目录的所有者为 test
$ chown test:test a.txt # 同时改变 a.txt 的所有者和所有组
```

### `cp` 命令

`cp` 命令用来复制文件或目录，意为 Copy。

```bash
$ cp 1.txt 2.txt # 复制 1.txt 到 2.txt
$ cp 1.txt test # 复制 1.txt 到 test 目录下，使用 -i 进行交互式覆盖
$ cp -a test1 test2 # 复制整个目录
```

### `cut` 命令

`cut` 命令用来显示行中的指定部分，接受标准输入或文件参数。

```bash
$ cat a.txt | cut -d ";" -f1 # -d 指定分隔符，默认为制表符，-f 为选择的字段
$ cut -b 1,3 a.txt # 显示指定字节
$ cut -c -4 a.txt # 显示前 4 个字符
$ cut -c -4 a.txt --complement # 显示除前 4 个字符以外的其他字符
```

### `date` 命令

`date` 命令用来显示或设定系统的日期与时间。

```bash
$ date # 输出当前时间
```

### `df` 命令

`df` 命令用来检查文件系统的磁盘空间占用情况。

```bash
$ df # 显示磁盘使用情况
$ df -h # 以更易读的方式显示目前磁盘空间和使用情况
```

### `diff` 命令

`diff` 命令用来比较文件内容，特别是比较两个版本不同的文件以找到改动的地方，是版本控制工具不可或缺的一部分。

```bash
$ diff 1.txt 2.txt # 比较 1.txt 和 2.txt 文件
$ diff test1 test2 # 比较两个目录
```

### `du` 命令

`du` 命令与 `df` 命令不同，其用来查看文件和目录的磁盘占用空间。

```bash
$ du # 递归显示目录和文件所占空间
$ du mydir # 递归显示 mydir 目录和所有文件所占空间
$ du a.txt # 显示 a.txt 占用空间
$ du -s # 只显示总和大小
$ du -h # 以更易读的方式显示
$ du -c a.txt b.txt # 查看 a.txt、b.txt 大小并统计总和
```

### `find` 命令

`find` 命令沿着文件层次结构向下遍历，匹配符合条件的文件，执行相应的操作。

```bash
$ find base_path # bash_path 可以是任意位置，find 将会从该位置开始向下查找
$ find . -name "*.txt" # 选项 -name 指定文件名必须匹配的字符串，还可以使用 -iname 忽略大小写
$ find . -path "*/aaa/*" # 选项 -path 可以使用通配符来匹配文件路径
$ find . -regex ".*\.sh" # 选项 -regex 使用正则表达式来匹配
$ find . ! -name "*.txt" # ! 表示函数的否定意义
$ find . -maxdepth 1 -name "f*" # find 命令在使用时会遍历所有的子目录，可以采用深度选项 -maxdepth 和 -mindepth 来限制遍历目录的深度，此项参数应尽量放在第三个参数的位置
$ find . -type d # 选项 -type 可以对文件根据文件类型进行过滤，d 表示目录，f 表示普通文件，l 表示符号链接，b 表示块设备，s 表示套接字
$ find . -atime -7 # 根据访问时间、修改时间、变化时间进行过滤，时间单位为天
$ find . -type f -size +2k # size 选项根据文件大小进行搜索
$ find . -type f -name "*.swp" --delete # delete 选项删除匹配到的文件
$ find . -type f -perm 644 # perm 选项根据文件权限进行匹配，user 选项文件所有者进行匹配
$ find . -type f -user root -exec chown user {} # -exec 用于执行命令，{} 是特殊字符串表示所有匹配到的文件名
$ find . \(-name ".git" -prune\) # prune 选项将某些文件或目录从搜索过程中排除，这叫做修剪
```

### `free` 命令

`free` 命令可以显示系统中空闲内存、已用的物理内存以及交换内存，是常用的监控工具（MacOS 上不存在）。

```bash
$ free # 显示内存使用情况
```

### `grep` 命令

`grep` 命令是非常强大的文本搜索工具，如果匹配到相关信息，就打印出符合条件的行，对正则有简单的支持。

```bash
$ grep [-ivnc] string file # 例如 grep -c 'name' a.log，
# -i: 不区分大小写
# -v: 反向匹配，即不包含指定模式
# -n: 同时输出匹配的行号
# -c: 只输出包含匹配的行数
```

### `gzip` 命令

`gzip` 是 Linux 系统中常用的对文件进行压缩和解压缩的命令，通常和 `tar` 命令结合使用，对文本文件有 60%~70% 的压缩率。

```bash
$ gzip * # 把当前目录下的每个文件都压缩成 `.gz` 文件
$ gzip -dv * # 解压当前目录下所有的 `.gz` 文件
$ gzip -l * # 显示每个压缩的文件信息，并不解压
```

### `head` 命令

`head` 命令就像它的名字一样，用来显示档案的开头。

```bash
$ head a.txt # 显示 a.txt 的前 10 行
$ head -n 5 a.txt # -n 指定显示的行数，在 Linux 行数为负数表示除最后几行的所有行
$ head -c 30 a.txt # -c 指定显示的字节数
```

### `kill` 命令

`kill` 命令用来终止指定的后台进程，是常用的进程管理工具，不过在使用时需要提前知道进程的 PID。

```bash
$ kill -l # 列出所有的信号名称
$ kill -l QUIT # 列出该信号的数值
$ kill 2222 # 杀死 PID 为 2222 的进程
```

### `killall` 命令

`killall` 命令用于杀死指定名称的进程，比 `kill` 命令更加简单。

```bash
$ killall vi # 杀死所有 vi 进程
```

### `less` 命令

`less` 命令是对文件或其他输出进行分页显示的工具，相比于 `more` 功能更强。

```bash
$ less a.txt # 分页查看文件，使用回车键到下一行，空格键到下一页
$ history | less # 分页显示命令历史记录，输入 `/str` 进行向下搜索，使用 `?str` 进行向上搜索，-i 参数忽略搜索大小写，键盘 n 和 N 表示重复上一个搜索和反向重复上一个搜索
$ less a.txt b.txt # 浏览多个文件，输入 n 切换到下一个文件，输入 p 切换到上一个文件
```

### `ln` 命令

`ln` 命令用于为某一文件在另外一个位置建立一个同步的链接，不会复制导致重复占用磁盘空间。

```bash
$ ln -s 1.txt link1 # 给 1.txt 创建软链接
$ ln -sv /opt/test /bin/test # 给目录创建软链接
$ ln 2.txt link2 # 给 2.txt 创建硬链接
```

### `mkdir` 命令

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

### `more` 命令

`more` 命令功能类似于 `cat`，不过 `cat` 会将整个文件内容从上到下显示在屏幕上。`more` 命令在启动时就加载整个文件，并会一页一页显示以方便阅读，使用空格键显示下一页，使用 _b_ 显示上一页，而且支持字符串搜索。

```bash
$ more +3 1.txt # 显示文件中从第 3 行起的内容
$ more +/text 1.txt # 从文件中查找第一个出现 text 字符串的行，并从该处前两行开始显示输出
$ ls -l | more -5 # 设定每屏显示的行数为 5 来显示当前目录下的所有文件
```

### `mv` 命令

`mv` 命令用来对文件或目录进行移动或重命名，表示 Move。

```bash
$ mv test.txt test.log # 将 test.txt 重命名为 test.log
$ mv test.log logs # 将 test.log 移动到 logs 目录中，logs 目录必须事先存在
$ mv -t ~/logs 1.log 2.log # 使用 -t 指定移动的目标目录，用来移动多个文件
$ mv -i 1.txt 2.txt # 如果 2.txt 存在，询问是否覆盖
$ mv -f 1.txt 2.txt # 直接用 1.txt 覆盖 2.txt
```

### `nl` 命令

`nl` 命令用来计算文件中的行数，相对于 `cat -n`，`nl` 命令可以做更多的显示设计。

```bash
$ nl test.txt # 同 `cat -n text.txt`
$ nl -v 0 test.txt # -v 指定行号从 0 开始
```

### `ps` 命令

`ps` 命令列出执行此命令时，系统中运行进程的快照（Process Status），该命令可以确定进程的大部分信息。

```bash
$ ps -a # 显示同一终端下的所有进程
$ ps -A # 显示所有进程
$ ps -u test # 显示指定用户的进程
$ ps aux # 显示所有正在内存中的程序 
```

### `pwd` 命令

`pwd` 意为 Print Working Directory，该命令用来输出工作目录路径。

```bash
$ pwd # 输出当前工作目录的完整路径
$ pwd -P # 如果目录是链接时，输出实际路径
```

### `rm` 命令

`rm` 命令用于删除文件和目录，是一个较为危险的命令，很多人在用此命令时通常会添加别名，并通过 `mv` 命令备份删除的文件，防止误删。

```bash
$ rm a.txt # 删除 a.txt 文件
$ rm -f a.txt # 强制删除，不询问
$ rm -r dir_a # 删除目录 dir_a，并递归删除其下的所有目录和文件
$ rm -i *.log # 交互式删除当前目录下的所有 .log 文件
$ rm -- -file # 删除 -file 文件
```

### `sed` 命令

`sed` 是一种流编辑器（Stream Editor），能够完美地配合正则表达式使用，是文本处理中的利器。

```bash
$ sed -n '10p' file.txt # 查看 file.txt 第 10 行
$ sed -n '3,5p' file.txt # 查看 file.txt 第 3-5 行
```

### `sort` 命令

`sort` 将文件的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按 ASCII 码值进行比较，最后将它们按升序输出。

```
$ sort -[ntkr] file
# -n: 采用数字排序
# -t: 指定分隔符，默认为单个空格、制表符
# -k: 指定要排序的列
# -r: 进行反向排序
```

### `split` 命令

`split` 命令用来将文件分割成多个更小的片段。

```bash
$ split -b 10k data.file -d -a 4 split_file # 把 data.file 文件以 10KB 大小分割，d 选项表示使用数字为后缀，a 选项表示后缀长度，split_file 为文件名前缀
$ split -l 500 data.file # 根据行数分割文件
```

### `tail` 命令

`tail` 命令就像它的名字一样，用来显示档案的结尾。

```bash
$ tail a.txt # 显示 a.txt 的最后 10 行
$ tail -n 5 a.txt # -n 指定显示的行数
$ tail -c 30 a.txt # -c 指定显示的字节数
$ tail -f a.log # -f 循环查看文件内容，当文件在改变时，此命令很有用
$ tail -n +5 a.txt # 从第 5 行显示文件
```

### `tar` 命令

`tar` 命令可以对文件进行归档和从归档中提取文件。

```bash
$ tar -cf output.tar file1 file2 folder1 # -cf 创建归档
$ tar -tvf output.tar # -tf 列出归档
$ tar -rf output.tar new_file # -rf 向归档中添加文件
$ tar -xf output.tar # -xf 从归档中提取文件或文件夹
$ tar -xf output.tar file1 folder1 -C /path/to/outdir # -C 从归档中提取特定文件到指定路径中去
$ tar -Af file1.tar file2.tar # -Af 合并多个归档到第一个中
$ tar -f output.tar --delete file1 # --delete 从归档中删除指定的文件
$ tar -acvf output.tar.gz file1 file2 folder1 # 根据扩展名自动压缩归档，其他的压缩格式选项有 -j、-z、--lzma
$ tar -cf output.tar * --exclude "*.txt" # 排除部分文件的归档
```

### `top` 命令

`top` 命令用于实时显示系统中各个进程的资源占用情况，输入 _q_ 退出。

```bash
$ top # 显示进程信息
```

### `touch` 命令

`touch` 命令用来新建一个文件或修改文件的时间戳。

```bash
$ touch 1.txt # 创建新文件 1.txt
```

### `tr` 命令

`tr` 命令可以对来自标准输入的内容进行字符替换、字符删除以及重复字符压缩，也叫做转换（translate）命令。

`tr` 命令只能通过标准输入，而无法通过命令行参数来接受输入，调用格式如下：

```bash
$ tr [options] set1 set2 # 将来自标准输入的字符从 set1 映射到 set2
$ echo HELLO WHO IS THIS | tr "A-Z" "a-z" # hello who is this
$ echo tr came, tr saw, tr conquered. | tr 'a-zA-Z' 'n-za-mN-ZA-M' # ROT13 加密
$ echo ge pnzr, ge fnj, ge pbadhrerq. | tr 'a-zA-Z' 'n-za-mN-ZA-M' # ROT13 解密
$ tr -d '1-3' < example.txt # 使用 d 选项删除指定的字符
   4 5 6
7 8 9 0
$ echo hello 1 char 2 next 4 | tr -d -c '0-9 \n' # 使用 c 选项指定使用字符集的补集
 1  2  4
$ echo "GNU is     not     UNIX. Recursive    right  ?" | tr -s ' ' # 使用 s 选项压缩重复的字符
GNU is not UNIX. Recursive right ?
```

### `wc` 命令

`wc` 命令用于统计文件中字节数、字数、行数，并将统计结果显示输出。

```bash
$ wc -l test.txt # 显示文件行数
$ wc -c test.txt # 显示文件字节数
$ wc -w test.txt # 显示文件字数，一个字被定义为由空白、跳格或换行字符分隔的字符串
$ wc -m test.txt # 显示文件字符数
$ wc -L test.txt # 打印最长行的长度
```

### `where` 命令

`where` 命令可以定位包括 `PATH` 环境变量，Shell 内置命令和自定义的函数、别名等。

```bash
$ where venv # =>
# venv () {
# 	python3.6 -m venv $*
# 	[ -f ~/.config/pip/pip.conf ] && cp ~/.config/pip/pip.conf ./venv
# }
```

### `whereis` 命令

`whereis` 命令是根据 `PATH` 环境变量定义的目录，来定位文件绝对路径的命令，功能与 `which` 类似，不过它还能找到相关的 `man` 文件。

```bash
$ whereis pwd # => /bin/pwd
```

### `which` 命令

`which` 命令是根据 `PATH` 环境变量定义的目录，来定位文件绝对路径的命令。

```bash
$ which ls # => ls: aliased to ls -G
$ which ee # => ee not found
$ which pwd # => pwd: shell built-in command
$ which npm # => /usr/local/bin/npm
$ which venv # venv() { echo "venv" }
```

### `xargs` 命令

有些命令只能以命令行参数的形式接受数据，而无法通过 stdin 接受数据流，这时就没法用管道来提供那些只有通过命令行参数才能提供的数据。`xargs` 命令就是为了解决这个问题的，它擅长将标准输入数据转换成命令行参数。

`xargs` 命令应紧跟在管道符之后，以标准输入作为主要的源数据流，并以此提供命令行参数来执行其他命令。

```bash
$ cat example.txt # 样例文件
1 2 3 4 5 6
7 8 9 10
11 12
$ cat example.txt | xargs 
1 2 3 4 5 6 7 8 9 10 11 12
$ cat example.txt | xargs -n 3 # 每行保存最多 3 个参数
1 2 3
4 5 6 
7 8 9
10 11 12
$ echo splitXsplitXsplitXsplit | xargs -d X # d 选项进行分割，在 MacOS 上需要使用 I 选项
$ find . -type f -name "*.txt" -print0 | xargs -0 rm -f # 删除当前目录下所有的 .txt 文件
$ find . -type f -name "*.c" -print0 | xargs -0 wc -l # 统计所有 C 程序文件的行数
```
