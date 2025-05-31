---
title: Hexo博客多机更新
tags: []
categories: []
poster:
  topic: null
  headline: null
  caption: null
  color: null
date: 2025-05-03 12:24:28
description:
cover:
banner:
sticky:
mermaid:
katex:
mathjax:
topic:
author:
references:
comments:
indexing:
breadcrumb:
leftbar:
rightbar:
h1:
type:
---




# Hexo的多机更新

正如大家所看见的样子，我的个人博客很少有时间更新；更准确的来讲就是我的文章产出是非常稀少的，
那么我说明一下，目前我在供稿的公众号平台主要是AIProtein、SDbioinfor以及我自己的AIbioMed日记（就是这么深爱这个学科）

虽然跟新的频率很慢，但是还是有很多有意思的文章可以来更新。那么自己的博客为什么更新的这么慢呢？
在看过了《孤独的美食家》之后又再次诱发了我的更新创作的灵感。

那么到底是什么阻挠了我呢？（~~绝对不是懒~~）

---

## 已有环境
Home_PC : Win11 + Git

Home_Server : Arch Linux + Git

Office : Win11 + Git

## Hexo框架多机更新的原理
主要是利用Git分支来实现hexo和源代码的同步

hexo生成的静态网页默认放在master分支上，这个由Hexo源代码文件夹下面的_config.yml文件

在全局配置中进行配置

```
deploy:
    type: git
    repo: git@github.com:username/username.github.io.git
    branch: master
```

那么我们之后创建一条hexo的分支，用以保存blog的源代码


## 更新的流程
那么之后我们每次进行文章的更新的时候，

同步的步骤

```
# 合并仓库和本地
git pull

# 同步源代码
git add .
git commit -m "xxxx"
git push

# 同步文章
hexo d -g 
```