---
title: Github入门教程
date: 2021-03-09 13:25:41
tags: 
---

## 写在前面

这又是我开的另一个大坑的内容，这部分主要是想要讲一下GitHub的基本使用方法、一些常用的指令以及一个我正在采用的同步的一个流程。

## 关于GitHub

GitHub是全球最大的同性交流网站，在上面你几乎能找到你想要的一切内容，而且越来越多人和组织加入到开源的大家庭中这是一件好事。我也是很早以前就接触到了开源的世界，大家在一起交流分享知识增进自己的代码水平，让自己不断的成长。

### 进入社区前你要做好的准备

首先就是一个GitHub的账号，当然如今市面上有很多的开源社区都可以选择，技术和实现的过程几乎是一样的，这里我以我最常用的GitHub为例子来个大家讲解。账号可以在GitHub首页直接注册，目前来说裸连的话也是没有问题的。但是有些时候也会抽风，所以最好准备一个梯子以防不时之需。

注册完账号之后便是配置在本地环境，直接在终端中输入以下指令

`ssh-keygen -C "email adress" -t rsa`

这条指令会在你的用户路径下生成一个./ssh的文件夹，进入之后你会发现有两个文件，id_rsa这个文件是私钥（保存好），另一个id_rsa.pub这个是你的公钥。

之后进入GitHub的个人设置页面中`SSH and GPG keys`选项中将你的公钥复制进去（是公钥里面的数据，记事本打开即可），完成之后点击add SSH key。

这样你的配置就完成了。

## 关于指令的用法

在本地的环境配置好了以后	


[常用指令](https://www.notion.so/d4a976e09955401fbf818fb9dc29a981)

## 创建新仓库

创建新的文件夹，打开，执行

```bash
git init
```

## 检出仓库

执行语句将他人的仓库克隆到本地

```bash
git clone /path/to/repository
```

如果是远程服务器上的仓库

```bash
git clone username@host:/path/to/repository
```

## 工作流

本地的仓库被GitHub用三部分进行维护，并加以三颗“树”状结构进行维护。

工作目录   是你仓库中实际存储的内容

暂存区（Index）   在你改动工作目录后，将你的操作暂时存储在该区域

HEAD     他指向自己的最后一次操作

## 添加和提交

可以提出更改（将他们添加到暂存区中）

```bash
git add <filename>
git add *
```

使用指令将实际的更改提交

```bash
git commit -m "代码提交信息"
```

之后你的改动就被提交到了HEAD，但是还没有推送到远程仓库

## 推送改动

执行语句，将你的改动从HEAD中推送到远端仓库

```bash
git push origin master
```

上方语句实例中，将HEAD提交到master分支中，也可提交到其他分支中。

如果没有克隆现有仓库，而且你还想将自己的仓库同步到某个服务器中

```bash
git remote add origin <server>
```

这样就可以将自己的仓库同步上去了

## 分支

在开发的时候是在默认的主支上进行的，你可以通过一定的指令操作同步到其他分支，最后开发结束之后合并在一起。

```bash
git checkout -b feature_x  //创建一个feature_x的分支，并切换过去
git checkout mastr         //切换到主分支
git branch -d feature_x    //将feature_x分支删除
git push origin <branch>   //除非将分支推送到远程仓库，不然别人无法访问
```

## 更新合并

更新本地仓库至线上最新改动版本

```bash
git pull
```

以用来在自己的工作目录中 *获取(fetch)* 并 *合并(merge)  远端的改动*

如果要合并其他分支到自己当前分支上时，执行

```bash
git merge <branch>
```

在这两种情况下，git都会尝试自动合并改动。有一定的几率会出现 冲突(conflicts) 。这时候需要自己去手动修改某些文件，使得修改完成之后，执行

```bash
git add <filename>
```

将修改的内容标记为合并成功

在合并之前，可以使用

```bash
git diff <source_branch><target_branch>
```

来看一下修改前后文件的差别在哪里

## 标签

为软件发布创建标签是推荐的，在之前的软件开发过程中，这个理念就有所体现，在SVN中也有。

可以执行以下命令创建一个叫做1.0.0的标签

```bash
git tag 1.0.0 1b2e1d63ff
```

后面的 1b2e1d63ff 是你想提交ID的前十位字符，可以使用 git log 获取提交ID

## log

如果想要了解一个本地仓库的历史记录，可以直接执行

```bash
git log
```

同时也可以添加一些参数，来获取一些自己想要的特定消息，只看某一人的提交

```bash
git log --author=bob
```

一个压缩后的每一条提交记录只占一行的输出：

```bash
git log --pretty=oneline
```

或者你想通过ASCII艺术的树形结构来展示所有的分支，每个分支都标示了他的名字和标签

```bash
git log --graph --oneline --decorate --all
```

看看那些文件发生了变化

```bash
git log --name-status
```

这些只是你可以使用的一小部分参数

```bash
git log --help
```

## 替换本地改动

假如操作发生失误，可以使用如下的命令替换掉本地改动

```bash
git checkout --<filename>
```

此命令可以将HEAD中的最新内容替换掉你的工作目录中的文件。已经添加到暂存区的改动以及新文件都不会收到影响。

```bash
git fetch origin
git reset --hard oringin/master
```

假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你的本地主分支指向它
