---
title: Linux命令行模式
urlname: linux-commond
date: 2016-07-21 17:31:04
categories: 涉猎
tags: [linux]
---
1 图形界面的开启与关闭

启用超级用户编辑启动配置文件`/etc/inittab`

修改第26行：

- 启动图形界面`id:5:initdefault:`

- 不启动图形界面`id:3:initdefault:`

2 修改终端中目录颜色为黄色


启用超级用户编辑配置文件`/etc/DIR_COLORS`

修改第81行`DIR 01;34`为`DIR 01;33`

同样，在这里也可以修改其他的文字颜色

*值得注意的是：Linux自带的配置文件都做了较为详细的注释，仔细阅读注释必定会有更多的收获*

3 Linux命令行翻屏

向上`Shift + Page Up`，向下`Shift + Page Down`

4 重启和关机

重启`reboot` 关机`poweroff`