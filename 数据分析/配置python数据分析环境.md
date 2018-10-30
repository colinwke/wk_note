title: 配置python数据分析环境
urlname: config-python-data-analysis-envir
categories: 数据分析
tags: [python, 数据分析]
date: 2016-10-17 17:40:40
---


1 python安装

1.1 下载
官方网站：https://www.python.org/
搜狐镜像：http://mirrors.sohu.com/python/

示例版本：python35
官方地址：https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe
搜狐镜像：http://mirrors.sohu.com/python/3.5.2/python-3.5.2-amd64.exe

提示：[下载版本的选择][1]

1.2 添加环境变量
注：在安装引导中，已默认勾选自动添加环境变量。


1.3 测试
```
cmd >> python
Python 3.5.2rc1 (v3.5.2rc1:68feec6488b2+, Jun 12 2016, 08:56:24) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

显示则配置成功。
若提示命令错误，则需手动添加环境变量，将python根目录添加到path中。

2 配置python数据分析依赖包

2.1 安装pip
pip（python package index，官方网址：https://pypi.python.org/pypi），是安装python依赖包的工具（通过pip检索得到依赖包地址）。
注：在安装python的引导中，已默认勾选并安装pip（跳至2.1.4）。

2.1.1 下载get-pip.py
下载地址：https://bootstrap.pypa.io/get-pip.py

2.1.2 运行get-pip.py
`cmd >> python get-pip.py`

2.1.3 添加环境变量
进入python根目录，会发现多了两个文件夹。
添加Script文件夹路径到环境变量。

2.1.4 测试与更新pip
测试：
`cmd >> pip`
打印pip指令表示配置成功。
更新：
`cmd >> python -m pip install --upgrade pip`
注：使用pip安装依赖包时，需要管理员权限！

2.1.5 pip镜像
由于多数依赖包存储在海外，频繁造成下载不稳定，可选设置pip镜像下载。
豆瓣镜像：http://pypi.douban.com/simple/
清华镜像：https://pypi.tuna.tsinghua.edu.cn/simple

临时使用：
`cmd >> pip install -i https://pypi.tuna.tsinghua.edu.cn/simple packagename`
一劳永逸：
在user目录下，创建pip目录（C:\Users\username\pip），新建文件pip.ini。
编辑内容如下：
```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

2.1.6 pip安装已编译包
一般情况下的pip安装（pip install packagename）是下载源文件后进行编译再安装，由于编译环境原因，有可能导致编译失败。
解决方法为下载已编译好的依赖包（*.whl）。
下载地址：http://www.lfd.uci.edu/~gohlke/pythonlibs/
`cmd >> pip install *.whl` 进行安装。
需要注意的是：依赖包命名中，cp后面数字为python版本，且必须下载对应python版本。

2.2 安装numpy+mkl
numpy（numerical python的简称）是python科学计算的基础包。
mkl（intel math kernel library）基于intel处理器的数学计算核心库。
使用2.1.6（pip安装已编译包）进行安装。

2.3 安装scipy
scipy是一组专门解决科学计算中各种标准问题域的包集合。numpy和scipy的结合可完全替代matlab的计算功能。
使用2.1.6（pip安装已编译包）进行安装。

2.4 安装matplotlib
matplotlib是最流行的用于绘制数据图表的python库。
`cmd >> pip install matplotlib`

2.5 安装pandas
pandas（panel data）提供了大量快速便捷处理结构化数据的数据结构和函数。
`cmd >> pip install pandas`

2.6 安装scikit-learn（可选）
scikit-learn是基于python的机器学习库。
`cmd >> pip install scikit-learn`

2.7 安装ipython
使用2.1.6（pip安装已编译包）进行安装。
或者：https://github.com/ipython/ipython/downloads

参考连接：
[How To Install Python, pip, and virtualenv on Windows with PowerShell](http://www.tylerbutler.com/2012/05/how-to-install-python-pip-and-virtualenv-on-windows-with-powershell/)
[Installing the SciPy Stack](https://www.scipy.org/install.html)
[更改pip源至国内镜像，显著提升下载速度](http://blog.csdn.net/lambert310/article/details/52412059)

[1]:http://zhidao.baidu.com/link?url=GqjYQTgAGXCyq7gz7XmcNzxr55-bU6T_uSRxCKuYEZSDqgR-Rz77bAKpFqJ07KizxYEW3CowBKociDDlA9j9pPGJ0ws81H0d-L6FpT993KK

anaconda 下载
清华大学开源软件镜像站
https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/

pip update all package in windows
https://stackoverflow.com/questions/2720014/upgrading-all-packages-with-pip

``` python
import pip
from subprocess import call

for dist in pip.get_installed_distributions():
    call("pip install --upgrade " + dist.project_name, shell=True)
```