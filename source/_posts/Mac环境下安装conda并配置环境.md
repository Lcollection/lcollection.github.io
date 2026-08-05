---
title: Mac环境下安装conda并配置环境
date: 2021-02-09 23:45:53
tags: [Conda] 
---

# 写在前面

  从Linux环境到Windows环境再到如今Mac环境下，<!--more-->python的环境和一些常用环境的配置都挺让人头大的，正巧最近了解到conda环境可以很方便的配置一些常用的环境，以及一些python的包管理也很方便，所以这里我便简单的记录一下我的安装历程，把出现的一些问题罗列一下，希望能帮助到别人不踩相同的坑。



# 安装Anacoda/Miniconda

## 下载安装



根据自己使用的环境的不同，可以在清华大学开源镜像站中找到合适的安装包，

[快速链接](https://mirrors.tuna.tsinghua.edu.cn/#) 在右侧获取下载链接 -----> 常用软件 ------> 选择anaconda/miniconda

两者的主要区别就是，anaconda包含了所有的所需要的内容，而miniconda是一个轻量级的替代，只包含了python和conda从而体积也小了很多。



下载之后在终端中，使用指令`bash xxxxx.sh`进行安装，按回车后会出现大段的安装说明，直接顺着流程进行就可以顺利安装了

## 配置conda环境

安装完成之后，使用`vim ~/.condarc`编辑这个文件，便可以修改里面的内容，根据清华大学开源镜像站中的帮助手册，你可以很快的配置完成这一步。

## 出现的情况

每次打开终端自动进入conda的虚拟环境

解决方法：

​			在.condarc中配置	auto_activate_base  为   false就是关闭

​			如果需要打开 conda config --set auto_activate_base true

​			就可以重新打开

# 创建虚拟环境

使用指令 `conda create -n env_name`可以创建一个名为env_name的环境

之后进入环境之后再运行 `conda install pkg_name`安装你所需要的包

## 出现的问题

1.

```

Linking packages … 
PaddingError: Placeholder of length ‘30’ too short in package qt-5.6.2-vc14_0.% 
The package must be rebuilt with conda-build > 2.0.

```

方案：

`conda update conda` ,然后`conda update –all `下，可以解决大部分问题

参考阅读：https:*//blog.csdn.net/xxzhangx/article/details/54379255?locationNum=3&fps=1*



# 常用的指令

## 退出虚拟环境

`conda deactivate`

## 查看所有虚拟环境

`conda info -e`

## 查看安装的包

`conda list`

## 删除虚拟环境

`conda remove -n env_name --all`

