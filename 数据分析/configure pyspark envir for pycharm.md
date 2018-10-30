---
title: 配置pyspark本地调试环境
urlname: config-local-pyspark-envir
categories: 数据分析
tags: 数据分析
date: 2017-9-9 21:32:53
---

1 下载: https://spark.apache.org/downloads.html
注: 不要追求最新的版本, 最新的版本意味着坑可能多.
示例版本: spark-2.1.1-bin-hadoop2.6.tgz

2 解压
注: 解压后的路径不能包含空格
(尤其注意`Program File`, 另外, spark依赖java环境, 因此java安装路径也不能有空格!)

3 配置
3.1 进入spark根目录下python文件夹, 复制pyspark文件夹至python根目录下`Lib\site-packages`文件夹内.
3.2 pycharm: Run/Edit Configurations/Defaults/Python/Configuration/Environment Variables/
add:
```
Name: SPARK_HOME
Value: spark_home_path
```
3.3
新建python脚本`spark_local.py`

``` python
from pyspark import SparkContext


if __name__ == '__main__':
    sc = SparkContext(appName='test')
    rdd = sc.textFile('./spark_local.py')
    rdd.foreach(print)
```

运行, 结果显示能够读取并打印文本内容, 但是也存在错误
```
17/09/09 22:10:36 ERROR Shell: Failed to locate the winutils binary in the hadoop binary path
java.io.IOException: Could not locate executable null\bin\winutils.exe in the Hadoop binaries.
	at org.apache.hadoop.util.Shell.getQualifiedBinPath(Shell.java:355)
	at org.apache.hadoop.util.Shell.getWinUtilsPath(Shell.java:370)
	...
	at py4j.GatewayConnection.run(GatewayConnection.java:214)
	at java.lang.Thread.run(Thread.java:745)
```
大致问题为没有找到本地的winutils.exe 运行文件
下载地址: https://github.com/steveloughran/winutils

1 下载对应版本的winutils.exe 添加到SPARK_HOME/hadoop/bin目录下(需要新建文件夹)
2 添加HADOOP_HOME环境
pycharm: Run/Edit Configurations/Defaults/Python/Configuration/Environment Variables/
add:
```
Name: HADOOP_HOME
Value: hadoop_home_path
```

另外, 也可以直接在代码里面配置SPARK_HOME 和HADOOP_HOME的运行环境
``` python
import os
import sys


def init_spark_local():
    """ init local spark envir """
    # Path for spark source folder
    SPARK_HOME = 'D:\_software_bak\spark\spark-2.1.1-bin-hadoop2.6'
    HADOOP_HOME = SPARK_HOME + '\hadoop'

    os.environ['SPARK_HOME'] = SPARK_HOME
    os.environ['HADOOP_HOME'] = HADOOP_HOME

    # You might need to enter your local IP
    # os.environ['SPARK_LOCAL_IP'] = '192.168.2.138'

    # Path for pyspark and py4j
    sys.path.append(SPARK_HOME + '\python')
    sys.path.append(SPARK_HOME + '\python\lib\py4j-0.9-src.zip')

```






