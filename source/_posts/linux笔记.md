---
title: linux笔记
date: 2020-08-18 12:31:49
tags:
---

# Linux 文件？设备？
<!--more-->
在 Linux 中，设备就是文件，文件夹也是文件。

# Linux 文件权限管理
简写 : 
$ r (Read) \\w (Write)\\ x (eXcute) \\s (Suid) \\t (sbiT) $
r 读文件(夹), w 写文件(夹), x 执行文件/进入文件夹


1. 查看文件权限

```sh
ls -l
```

eg.1 $ \rm drwxrwxrwx $

![drwxrwxrwx](https://gitee.com/inkuniverse/picture_bed/raw/master/img/20200818124241.png)

这是一个开放的文件夹。

2. 修改文件权限

```sh
chmod [权限数字] [文件(夹)名]
```

权限数字的计算

r: 4 w: 2 x: 1

rwx rwx rwx = (4+2+1)(4+2+1)(4+2+1) = 777

suid和sgid,sbit
适用于可执行文件,执行时获得特权/ bit自己创建的，其他非 root 无法修改。

3. 修改所属 
   用户： chown
   组： chgrp


# 常用命令

```sh
ls -l  # 文件属性
ls -la # 所有文件属性
cal # 日历
rm [-rif] # 删除文件
cp [-ar] # 复制文件
mv [-ar] # 移动文件
xc # 计算器
```