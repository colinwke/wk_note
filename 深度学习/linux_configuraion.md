---
title: linux环境配置
urlname: linux-envr-configuraion
categories: linux
tags: linux
date: 2018-8-31 21:17:23
mathjax: false


---

### 代理上网

- windows安装`CCproxy`

- ubuntu设置环境变量

  - 运行`vim /etc/profile`

  - 添加下面文本到末尾, `XXX.XX.XX.XX`为你代理服务器的ip地址

    - ```
      # proxy envir
      MY_PROXY_URL=http://XXX.XX.XX.XX:808/
      export ftp_proxy=${MY_PROXY_URL}
      export http_proxy=${MY_PROXY_URL}
      export https_proxy=${MY_PROXY_URL}
      
      # proxy envir or
      sudo apt-get -o Acquire::http::proxy="http://XXX.XX.XX.XX:808/" update
      ```

  - 运行 `source /etc/profile`更新

- 注意: 设置了代理可以安装软件和pip pakage, 但是仍然是不能ping通互联网的

### pip linux 安装代理

运行

```
mkdir ~/.pip
cd ~/.pip
vim pip.conf
```

添加下面内容

```
[global] 
index-url = http://pypi.douban.com/simple 
[install] 
trusted-host=pypi.douban.com
```

参考: https://blog.csdn.net/yucicheung/article/details/79095742

### Ubuntu设置静态IP

先查看目前网络配置

运行: `ifconfig`. 显示大概如下

```
enp0s25   Link encap:Ethernet  HWaddr 98:90:96:cb:14:9b  
          inet addr:172.23.21.60  Bcast:172.23.21.127  Mask:255.255.255.128
          inet6 addr: fe80::59f6:8ae2:9a3e:be83/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:289661 errors:38 dropped:0 overruns:0 frame:34
          TX packets:144345 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:377222113 (377.2 MB)  TX bytes:15347805 (15.3 MB)
          Interrupt:20 Memory:fb200000-fb220000 

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:424 errors:0 dropped:0 overruns:0 frame:0
          TX packets:424 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:36456 (36.4 KB)  TX bytes:36456 (36.4 KB)
```

- 第一行最左边的字符串, 为本机的网络接口, 不同机器可能不同!

运行: `vim /etc/network/interfaces`

在末尾添加:

```
auto enp0s25
iface enp0s25 inet static
address 172.23.21.60
netmask 255.255.255.128
gateway 172.23.21.1
dns-nameservers 8.8.4.4 8.8.8.8
```

- 注意
  - 网络接口要与之对应
  - dns-nameservers 好像是通用的, 不用修改

运行: `/etc/init.d/networking start`. 重启网络.

参考: https://askubuntu.com/questions/766131/set-static-ip-ubuntu



### Ubuntu apt-get 清华镜像

- https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/

运行: `vim /etc/apt/sources.list`

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
```

参考:

- https://blog.csdn.net/TotoroCyx/article/details/79517202

### 远程连接

没有测试!!

https://askubuntu.com/questions/592537/can-i-access-ubuntu-from-windows-remotely

### apt-get remove

https://askubuntu.com/questions/187888/what-is-the-correct-way-to-completely-remove-an-application

### pycharm 远程项目

https://blog.csdn.net/butcher8/article/details/40263069

finding your anaconda python interpreter path

- `which python`
- https://docs.anaconda.com/anaconda/user-guide/tasks/integration/python-path/

#### remoteterminal

tools -> start ssh session -> remote python

- https://www.jetbrains.com/help/pycharm/tutorial-using-the-product-built-in-ssh-terminal-and-remote-ssh-external-tools.html

### linux git error

使用ssh终端运行`git clone git@github.com:colinwke/wktk.git`时, 卡在

```
root@ss-Precision-Tower-7810:~# git clone git@github.com:colinwke/wktk.git
Cloning into 'wktk'...
/etc/ssh/ssh_config line 55: Unsupported option "GSSAPIAuthentication"
/etc/ssh/ssh_config line 56: Unsupported option "GSSAPIDelegateCredentials"
```

在网上搜索了关于`GSSAPIAuthentication`, `GSSAPIDelegateCredentials`的相关问题并没有得到解决.

并且直接在linux服务器上执行没有任何问题.

继续等待执行得到下面的完整结果:

```
root@ss-Precision-Tower-7810:~# git clone git@github.com:colinwke/wktk.git
Cloning into 'wktk'...
/etc/ssh/ssh_config line 55: Unsupported option "GSSAPIAuthentication"
/etc/ssh/ssh_config line 56: Unsupported option "GSSAPIDelegateCredentials"
ssh: connect to host github.com port 22: Connection timed out
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
root@ss-Precision-Tower-7810:~# 
```

解决方法是把`git`换为`https`

- 参考: 
- https://stackoverflow.com/questions/15589682/ssh-connect-to-host-github-com-port-22-connection-timed-out
- https://stackoverflow.com/questions/21634090/git-has-a-long-delay-between-git-pull-and-asking-for-password

### linux add python *.pth

```
run: python
import sys
print(sys.path)
```

get site-package path: `/root/anaconda3/lib/python3.6/site-packages`

add a `.pth` 

### Tensorflow Error

```python
ssh://root@172.23.21.60:22/root/anaconda3/bin/python -u /tmp/pycharm_project_262/tf_version_test.py
Traceback (most recent call last):
  File "/root/anaconda3/lib/python3.6/site-packages/tensorflow/python/pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "/root/anaconda3/lib/python3.6/site-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "/root/anaconda3/lib/python3.6/site-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "/root/anaconda3/lib/python3.6/imp.py", line 243, in load_module
    return load_dynamic(name, filename, file)
  File "/root/anaconda3/lib/python3.6/imp.py", line 343, in load_dynamic
    return _load(spec)
ImportError: libcublas.so.9.0: cannot open shared object file: No such file or directory

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/tmp/pycharm_project_262/tf_version_test.py", line 6, in <module>
    import tensorflow
  File "/root/anaconda3/lib/python3.6/site-packages/tensorflow/__init__.py", line 22, in <module>
    from tensorflow.python import pywrap_tensorflow  # pylint: disable=unused-import
  File "/root/anaconda3/lib/python3.6/site-packages/tensorflow/python/__init__.py", line 49, in <module>
    from tensorflow.python import pywrap_tensorflow
  File "/root/anaconda3/lib/python3.6/site-packages/tensorflow/python/pywrap_tensorflow.py", line 74, in <module>
    raise ImportError(msg)
ImportError: Traceback (most recent call last):
  File "/root/anaconda3/lib/python3.6/site-packages/tensorflow/python/pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "/root/anaconda3/lib/python3.6/site-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "/root/anaconda3/lib/python3.6/site-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "/root/anaconda3/lib/python3.6/imp.py", line 243, in load_module
    return load_dynamic(name, filename, file)
  File "/root/anaconda3/lib/python3.6/imp.py", line 343, in load_dynamic
    return _load(spec)
ImportError: libcublas.so.9.0: cannot open shared object file: No such file or directory


Failed to load the native TensorFlow runtime.

See https://www.tensorflow.org/install/install_sources#common_installation_problems

for some common reasons and solutions.  Include the entire stack trace
above this error message when asking for help.

Process finished with exit code 1

```

```
ImportError: libcublas.so.9.0: cannot open shared object file: No such file or directory
```

查看安装cude的版本号`apt list --installed | grep "cuda"`

显示版本没有问题.

在pycharm中添加环境变量, 解决!

LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64

- https://github.com/tensorflow/tensorflow/issues/9530

