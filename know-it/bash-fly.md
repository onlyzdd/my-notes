# 让你的 Bash 飞起来

## Bash 快捷键（Mac 版）

以下，需要设置 Option 键为 Meta 键，具体方式：终端 - 偏好设置 - 键盘 - 将 Option 键作为 Meta 键。

- Ctrl + A：移到命令行首
- Ctrl + E：移到命令行尾
- Ctrl + F：按字符向前移动（Forward），也可以使用左方向键
- Ctrl + B：按字符向后移动（Back），也可以使用右方向键
- Option + F：按单词向前移动（Forward），也可以使用左方向键
- Option + B：按单词向后移动（Back），也可以使用右方向键
- Ctrl + U：从光标处删除至命令行首
- Ctrl + K：从光标处删除至命令行尾
- Ctrl + W：从光标注删除至单词首，与 Option + Backspace 类似
- Option + D：从光标处删除至单词首
- Ctrl + D：删除光标处的字符
- Ctrl + H：删除光标前的字符
- Option + C：从光标处更改为首字母大写的单词
- Option + U：从光标处更改全部大写的单词
- Option + L：从光标处更改全部小写的单词
- Ctrl + T：交换光标和前一个字符
- Option + T：交换光标和前一个单词
- Option + .：使用上一条命令的最后一个参数
- Ctrl + P：历史中的上一条命令，也可以使用上方向键
- Ctrl + N：历史中的下一条命令，也可以使用下方向键
- Ctrl + R：逆向搜索命令历史，使用 Ctrl + G 退出搜索
- Ctrl + L：清屏
- Ctrl + C：终止命令
- Ctrl + Z：挂起命令

### 命令的返回值

当一个命令发生错误并返回时，会返回一个非 0 的退出状态；而当命令成功完成后，会返回数字 0。退出状态可以从特殊变量 `$?` 中获得。

### 命令序列

命令序列允许我们在不同情况下执行命令。

```
$ cmd1 ; cmd2   # 不管前面的命令是否执行成功，后面的命令都会继续执行
$ cmd1 & cmd2   # 命令同时执行
$ cmd1 && cmd2  # 前面的命令执行成功后才会执行后面的命令
$ cmd1 || cmd2  # 前面的命令执行失败后才会执行后面的命令
```

### Unix/Linux 时间戳

Unix/Linux 文件系统中每个文件都有三种时间戳：

- 访问时间 Atime：用户最近一次访问文件的时间
- 修改时间 Mtime：文件内容最后一次被修改的时间
- 变化时间 Ctime：文件元数据最后一次改变的时间

## 常用命令

### `cat` 命令

`cat` 命令常用语读取、显示或拼接文件内容，也叫做拼接（concatenate）命令。

```bash
$ cat file1 file2 file3 # 显示多个文件拼接后的内容
$ echo "Text through stdin" | cat - file.txt # 使用管道符从标准输入中读取，并将其与 file.txt 进行合并，- 被作为 stdin 文本的文件名
$ cat -s file.txt # 删除输出中多余的空白行，连续的空白行只显示一个
$ cat -n file.txt # 在每一行输出前加上行号，可以通过 -b 选项取消对空白行的编号
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

### `sort` 命令

`sort` 将文件的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按 ASCII 码值进行比较，最后将它们按升序输出。

```bash
$ cat example.txt # 样例数据
10 mac 2000
2 win 4000
4 bsd 1000
3 linux 1000
$ sort example.txt # 排序
10 mac 2000
2 win 4000
3 linux 1000
4 bsd 1000
$ sort -nr example.txt # 按数字逆序
10 mac 2000
4 bsd 1000
3 linux 1000
2 win 4000
$ sort -k 2 example.txt | uniq # 根据第二列排序，并只显示一次重复行
4 bsd 1000
3 linux 1000
10 mac 2000
2 win 4000
```

### `split` 命令

`split` 命令用来将文件分割成多个更小的片段。

```bash
$ split -b 10k data.file -d -a 4 split_file # 把 data.file 文件以 10KB 大小分割，d 选项表示使用数字为后缀，a 选项表示后缀长度，split_file 为文件名前缀
$ split -l 500 data.file # 根据行数分割文件
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

