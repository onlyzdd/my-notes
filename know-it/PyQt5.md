# PyQt5

## 项目依赖

```
altgraph==0.15
certifi==2018.4.16
chardet==3.0.4
future==0.16.0
idna==2.6
macholib==1.9
pefile==2017.11.5
PyInstaller==3.3.1 # 打包，生成桌面程序
PyQt5==5.10.1 # PyQt5
PyQtChart==5.10.1 # 画图用
requests==2.18.4 # 发请求用
sip==4.19.8
urllib3==1.22
```

## 应用框架

一般性的应用应该有类似下面的框架：

- [page].ui：页面显示 XML 描述文件，一般由 QtDesigner 软件生成
- [page].py：根据 .ui 文件由 PyUic 自动生成的布局文件
- app.py：逻辑控制文件，添加各种逻辑

## 运行应用

首先，请创建并激活虚拟环境（务必使用 Python3.5，防止出现打包问题），在虚拟环境中执行 `pip install -r requirements.txt` 以安装项目依赖。

直接输入命令：

```bash
(.venv)$ python app.py
```

## 应用打包

```bash
(.venv)$ pyinstaller --windowed --onefile --cleaned --icon=app.icns app.py
```

请注意，在 MacOS 上，应用图标必须使用 .icns 格式，可以去网站 [https://iconverticons.com/online/](https://iconverticons.com/online/) 上将其他格式转换为这种格式。

## 注意事项

一般的包都比较小，导出的桌面程序也差不多在 20M 的样子，但是如果需要使用 WebView 的技术，就会导入很大的包，生成的桌面程序就会达到 60M。
