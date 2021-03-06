# 使用 Make

_Make_ 诞生于 1977 年，是最常用的自动构建工具，主要用于 C 语言的项目。其从 _Makefile_ 中读取依赖关系，并从源代码中构建可执行程序或库。

在运行任何 _Make_ 目标之前，_Make_ 必须要知道该目标的生成规则，即依赖和动作。依赖是指构建目标时必须满足的条件，动作是指构建目标时执行的 Shell 命令。像这样的规则，通常写在一个名为 _Makefile_ 的文件中，_Makefile_ 也写作 _makefile_，也可以通过命令行参数指定为其他文件：

```sh
$ make -f rules.txt
$ # 或者
$ make --file=rules.txt
```

当目标（target）在 _Makefile_ 中时，就可以通过 `make <target>` 命令构建。

```sh
$ make # 无目标，构建 Makefile 文件中的第一个目标
$ make clean # 构建 Makefile 文件中的 clean 目标
```

## _Makefile_

所有的构建规则都写在 _Makefile_ 文件中，在使用 _make_ 命令前，此文件必须存在。对 _Makefile_ 文件的阅读和编写能力也很重要。

### 规则

_Makefile_ 文件由一系列规则构成，规则的形式如下：

```
<target>: <prerequisites>
    <commands>
```

上面第一行冒号前面的部分，叫做“目标”，冒号后面的部分叫做“前置条件”，第二行由 Tab 或 4 个空格开始，其后跟着“命令”。

目标是必需的，不可省略；前置条件和命令都是可选的。

### 目标

目标通常是文件名，指明 _Make_ 所要构建的对象。目标可以是多个文件名，之间用空格分隔。例如：

```
a.txt: b.txt c.txt
    cat b.txt c.txt > a.txt
```

除了文件名，目标还可以是某个操作的名字，这称为“伪目标”（phony target）。例如：

```
clean:
    rm *.o
```

但是如果当前目录中存在一个名为 _clean_ 的文件，那么 `make clean` 就不会执行了。这是因为 _Make_ 发现 _clean_ 文件已存在，不会尝试去构建。为了避免这种情况，可以明确声明 `clean` 是一个伪目标，这样 _Make_ 就不会去检查 _clean_ 文件了。

```
.PHONY: clean
```

### 前置条件

前置条件通常是一组文件名或其他目标，之间用空格分隔，它制定了目标的构建前提。如果前置条件不满足，目标构建就会失败。如果前置条件有过更新，目标就需要重新构建（通过前置条件中文件的上次修改时间戳，伪目标不存在此概念）。

如果前置条件中的某个文件不存在，那么就必须编写一条规则，来生成该文件。

### 命令

命令由一行或多行的 Shell 命令组成，它是构建目标的具体指令，它的运行结果通常就是生成目标文件。每行命令前，都必须有一个 Tab 或 4 个空格，这可以用内置变量 `.RECIPEPREFIX` 声明（此内置变量仅在 3.8.2 版本后可用）：

```
.RECIPEPREFIX = >
all:
> echo Hello, world
```

需要注意的是，每行命令在一个单独的 Shell 中执行，这些 Shell 之间没有任何继承关系。针对这个问题有 3 种解决方案：

1. 将多行命令写在一行，中间用分号分隔。
2. 在换行符前加反斜杠进行转义，这样下一行命令和当前行处于同一个 Shell 中，此种方法使用较多。
3. 使用内置的 `.ONESHELL` 命令，需在目标上一行指定。

## _Makefile_ 语法

### 注释

在 _Makefile_ 中使用 `#` 表示注释，注释 `#` 之后的所有内容。

### 回声

正常情况下，_Make_ 会打印每条命令，然后再执行，打印命令的过程就叫做回声。在命令的前面加上 `@`，可以关闭该命令的回声。

```
test:
    @echo This is a echo!
```

### 通配符

_Makefile_ 的通配符与 Bash 一致，包括 `*`、`?`、`...` 等，用来指定一组符合条件的文件名。

### 模式匹配

_Makefile_ 允许对文件名进行类似正则运算的匹配，主要用到的匹配符是 `%`。

### 变量

- 内置变量：_Make_ 提供了一系列的内置变量，通过 `$(var)` 来调用。
- 自定义变量：_Makefile_ 提供了 4 中赋值运算符 `=`、`:=`、`?=` 和 `+=`，来支持自定义变量，并在命令中通过 `$(var)` 来调用。
- Shell 变量：在调用 Shell 变量时，需额外增加一个 `$` 符号，以解决 _Make_ 的自动转义，例如 `echo $$HOME`。

### 自动变量

自动变量的值与当前规则有关，即根据规则对其进行赋值。

### 判断和循环

_Make_ 使用 Bash 语法，完成判断和循环。

### 函数

_Make_ 支持使用函数，调用方法如下：

```
$(function args)
# 或者
${function args}
```

_Make_ 中提供了许多内置函数，常用的包括：

- `shell` 函数：用来执行 Shell 命令
- `wildcard` 函数：用来替换 Bash 的通配符
- `subst` 函数：用来进行文本替换
- `patsubst` 函数：用于模式匹配的替换

## 其他

对于一个复杂的项目，除了 _Makefile_ 这一个文件，可能还存在 _configure_（也可以是其他的名字） 或 _CMakeLists.txt_ 文件。下面将介绍这些文件的目的，以及使用问题。

- 首先，这两种文件都是用来检测安装平台的目标特征，通常用来生成正确的 _Makefile_ 文件，以及对安装进行控制。
- _configure_ 就是一个普通的 Shell 脚本，通过 `./configure` 调用，通常使用参数 `--prefix=PATH` 来指定安装路径，并生成本地化的 _Makefile_ 文件。
- _CMakeLists.txt_ 是 _CMake_ 需要的文件，_CMake_ 是为了解决不同平台下 _Make_ 的差异所设计的。_CMakeLists.txt_ 文件定制了整个编译流程，通过 `cmake PATH` 调用，`PATH` 是 _CMakeLists.txt_ 文件所在的目录，然后生成本地化的 _Makefile_ 文件。
- 一般而言，一个项目中不会同时出现 _configure_ 和 _CMakeLists.txt_ 文件的。相对于 _configure_ 的方案，用 _CMake_ 做跨平台会更好。

所以，在一个复杂项目中正确编译和构建的方法应该是：

```sh
$ ./configure # 如果在当前目录中存在 configure 文件
$ cmake . # 如果在当前目录中存在 CMakeLists.txt 文件
$ make # 进行编译，运行前，Makefile 已经生成了
$ make install # 执行安装，此命令经常需要管理员权限
```

> 特别提示：当编译的某一步失败时，一般需要查看下是哪里出现了问题，如果要忽略错误继续执行，使用 `make` 命令的 `-i`（`--ignore-errors`） 标志。
