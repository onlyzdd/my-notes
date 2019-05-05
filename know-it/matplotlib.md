# Python 绘图库 Matplotlib 入门

这篇博客是干啥子的。

## 准备工作

### Matplotlib 简介

Matplotlib 是一个 Python 2D 绘图库，可以用在 Python 脚本、IPython 及 Jupyter Notebook交互环境，并支持生成多种图片格式。只需要少量的代码，就可以创建丰富的图表，可以完全掌控图表的样式，使用起来也非常简便。

### 安装

推荐使用 Conda 基本环境，或使用 `pip` 包管理器进行安装。

```bash
$ conda install matplotlib # or python -m pip install -U matplotlib
```

### 图表的组成元素

![](https://matplotlib.org/_images/anatomy.png)

### Matplotlib、pyplot 以及 pylab 之间的关系

Matplotlib 是整个包，pyplot 和 pylab 都是包中的模块。pylab 目前已不推荐使用，推荐使用 pyplot 和 numpy 的组合。鉴于此，以下将只讲解 pyplot 和 numpy 的基础使用。

### 后端

由于 Matplotlib 要支持多种运行环境，所以就需要能够输出多种格式结果的能力，这里的每种能力就叫做一种后端，而一套通用的代码就是前端了。

## 使用

### 基本操作



### 交互模式

当运行在交互式环境中时，通过开启交互模式，允许即时显示和刷新图形。这种交互模式的开启和关闭方法分别为 `plt.ion()` 和 `plt.ioff()`。


