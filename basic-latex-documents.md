# Latex 文档

## 到底什么是 Latex？

最基本的 Tex 程序可以完成简单的排版操作和程序设计功能，Leslie Lamport 开发的 Latex 是构建与 Tex 上，使用最为广泛的 Tex 格式，使得使用者可以在短时间内生成高质量的文档。对于生成复杂的数学公式，Latex 表现地更为出色。

Latex 是一种标记语言，经过处理可以生成多样的高质量文档，处理软件包括 TeXLive（推荐）、MiKTeX、CTeX 等。

## 源文档

Latex 源文件是简单的文本文件，可以使用任何的编辑器来创建，推荐编码为 UTF8。

### 文档类

Latex 源文件中通过 `\documentclass` 声明文档类，其中 `{}` 表示文档类参数（不可省略），`[]` 表示文档选项。`\begin{document}` 和 `\end{document}` 声明一个 document 环境，其间即是需要排版的内容。`\documentclass` 和 `\begin{document}` 之间为导言区，通常保存一些方便的全局配置，这些配置被称为**宏包**。

以

```tex
\documentclass{article}
\begin{document}
\end{document}
```

为例，使用的就是默认的文档类 `article`，这将引用 `article.cls` 类文件（此种内置的文档类，会自动引入），其中定义了一些命令，这些命令使你的文档变得结构化，将控制格式和页面布局。

内置的文档类有：

| 类型 | 介绍 |
| :--- | :--- |
| article | 期刊文章，由节 section 组成 |
| proc | 会议论文集，基于多篇 article |
| minimal | 最少设置的一种，只有基本字体和纸张大小设置，通常用于调试 |
| report | 报告，有章 chapter，节 section 等 |
| book | 书，有章 chapter，节 section 等 |
| slides | 幻灯片，使用较大的无衬线字体 |
| memoir | 非常灵活，可以基于此快速创建所需要的类型 |
| letter | 信件 |

相关的文档类选项有：

内置的文档类有：

| 选项 | 介绍 |
| :--- | :--- |
| 10pt, 11pt, 12pt | 设定文档使用的字号。默认是 10pt。 |
| a4paper, letterpaper,... | 定义纸张大小。除此之外，还有 a5paper, b5paper, executivepaper, legalpaper |
| fleqn | 独行公式向左对齐 |
| leqno | 公式编号列于左边 |
| titlepage, notitlepage | 指示文档标题之后的内容是否新开一页。article 类型无此选项 |
| onecolumn, twocolumn | 单栏，双栏 |
| twoside, oneside | 双边，单边。article 和 report 默认使用 oneside，book 默认为 twoside |
| landscape | 页面横放 |
| openright, openany | 与单双边设定和 \chapter 命令有关。report 默认 openany，book 默认 openright |
| draft | 以一个小黑块表明断行错误处，用于调试 |

更多关于文档类的介绍及定制文档类的相关信息请查看：[如何写一个LaTeX类文件，并设计你自己的简历](https://www.cnblogs.com/super-zhang-828/p/7450133.html)。

### 输入

连续的空格被认为是一个空格。行首的空格通常被忽略。按下回车产生的断行也被认为是空格。**一个空行**意味着一段的结束。连续的空行被当作是一个空行。

如下的符号在 LaTeX 中有特别的用处，
`# $ % ^ & _ { } ~ \`
一般它们不会被打印出来，如果需要显示，就输入
`\# \$ \% \^ \{ \} \& \_ \{ \} \~ \textbackslash`
来替代。

### 命令

命令通常由 `\` 开始，加上一串字母作为名字， 以空格，数字或者其他非字母字符结束。一些命令需要参数，用 `{ }` 引入。有时候需要选项，写在 `[ ]` 中。

```tex
\commandname[option1,option2,...]{argument1}{argument2}...
```

通过 `\newcommand{name}[num]{definition}` 来创建新的命令，`name` 是命令名称，`definition` 是定义，可选项 `num` 设定命令需要的参数 （最多为 9，默认是 0）。

新建命令不能和现有命令同名，如果要覆盖现有命令，通过 `\renewcommand`，用法跟 `\newcommand` 一致。另外还可以使用 `\providecommand`，在覆盖原有命令时，将不会产生任何警告。

### 环境

环境的作用和命令类似，但是作用于大段的文本。在 `\begin` 和 `\end` 之间可以任意使用其他命令或者环境。通过 `\newenvironment{name}[num]{before}{after}` 来创建新环境，`before` 和 `after` 包含环境内文本被处理之前和之后的一些命令。

```tex
\newenvironment{dotted} 
{\ldots{}}%
{\ldots{}}%
\begin{dotted}
 I am here
\end{dotted}
```

### 注释

注释以 `%` 区分。注释内容不会被排版。因为行首的空格会被忽略，所以如果需要断行而且断行的地方不要空格，可以放一个 `%`。在宏定义当中会经常见到。
大块的注释内容需要很多 `%`，操作起来不太方便，这时候，就可以利用 TeX 的条件语句，在其前面和后面写。

```tex
\iffalse
what are commented out
\fi
```

## 编译

编译是通过执行命令进行的，键入 `latex foo.tex` 回车，将会调用 latex 引擎编译 `foo.tex` 为 `foo.dvi`，接下来通过 `dvips foo.dvi && ps2pdf foo.ps` 就可以产生 PDF 文档。

编译殷勤有很多中，另外还有 `PDFLatex`、`XeLatex` 等工具将直接输出 PDF 文档，而不是用转换工具。对于中文文档而言，使用 `XeLatex`，其较好地提供了对中文的支持。例如：`xelatex foo`，将会自动寻找名为 `foo.tex` 的源文件进行编译。

## 扩展名及介绍

当你拿到一个 Latex 模板时，在编译之后会得到非常多的文件，下面对这些文件及其作用进行简单的介绍。

| 扩展名 | 描述 |
| :--- | :--- |
| .tex | Tex 源文件 |
| .sty | Latex 样式文件，使用 `\usepackage` 命令导入 |
| .dtx | Documented Tex，Latex `.sty` 文档的分发形式 |
| .ins | `.dtx` 的安装文件，`latex foo.ins` 会生成所需的 `.sty` 文档和帮助手册 |
| .cls | 类文档，使用 `\documentclass` 命令导入 |
| .fd | 字体描述文档 |
| .dvi | 设备无关的文档，由编译产生 |
| .pdf | PDF 文档 |
| .log | 编译日志文件 |
| .toc | 存放章节名称，在下一次编译时读入以产生目录 |
| .lot | 产生表格目录 |
| .aux | 存放各种标签引用信息，下次编译时读如以产生交叉参考 |
| .idx | 存放索引词，用 `makeindex` 处理 |
| .ind | 处理过的 `.idx` 文档，下次编译时读入以产生索引 |
| .ilg | `makeindex` 的日志 |

## 宏包

宏包是对现有文档类的各种修改、增补的集合。[CTAN](http://www.ctan.org/) 是一个在线的宏包档案库。其中主要的宏包已经包含在各种发行版里，请看[常用宏包](http://wiki.ctex.org/index.php/LaTeX/%E5%B8%B8%E7%94%A8%E5%AE%8F%E5%8C%85)。要创建自己的宏包，请参考[类与宏包写作](http://wiki.ctex.org/index.php?title=%E7%B1%BB%E4%B8%8E%E5%AE%8F%E5%8C%85%E5%86%99%E4%BD%9C&action=edit&redlink=1)。

### 使用现有的宏包

在文档**导言区**使用以下代码导入：

```tex
\usepackage[option]{package_name}
```

如果没有冲突，多个宏包也可以使用以下代码引入：

```tex
\usepackage{package1,package2,package3}
```

### 安装额外的宏包

额外的宏包建议去 [Tex Catalogue](http://texcatalogue.sarovar.org/) 查找，下载。根据宏包中的 `readme.txt` 这类说明文件，进行安转配置即可。通常需要经过的步骤是：

1. `latex foo.ins`：生成 `.dtx` 文档所需的文件
1. `latex foo.dtx`：生成 `.dvi`、`.pdf` 和 `.sty` 格式文件

将生成的 `.sty` 文件直接复制到源文档目录下即可。

## 注意事项

### 安装中文问题

安装推荐使用 TexLive，其在各个平台都有发布版本，而且自动安装了非常多的宏包，使用起来更加方便。

编译器推荐使用 XeLatex，其很好地提供了对中文的支持，此外 CTex、CJK 的方式也是可行的。

### 大型文档

处理大型的文档，一个有效的方式是把它分成几个部分，然后分别导入。

```tex
\include{filename1}
\input{filename2}
```

区别在于，`\include` 总是开始新的一页，`\input` 连续不分页。所以 `\include` 适合 book 类按 chapter 分割。

### 编辑器

推荐使用 `VSCode + Latex Workshop`，能够提供极强的自动补全，并自动生成文档大纲，其他传统的编辑器都是可以的。

### 用途

- 扩展 Markdown，使用 Latex 公式
- 编写论文，通常只需下载相应的模板，修改即可
