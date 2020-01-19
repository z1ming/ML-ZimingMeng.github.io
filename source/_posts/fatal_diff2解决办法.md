---
title: fatal:unknown style 'diff2' given for 'merge.conflictstyle'解决办法
date: 2019-11-06 11:00:00
author: Ziming M
top: false
cover: true
summary: 本文提供了使用git checkout切换新分支时出现问题的解决办法
categories: Git
tags:
  - Git
  - checkout
  - diff2
---
# 问题描述
新建一个分支，例如我的新分支名称为```newbranch```，当使用```git checkout newbranch```切换到新分支时，出现如下错误：
```
77392 (master) new-git-project
$ git checkout newbranch
fatal: unknown style 'diff2' given for 'merge.conflictstyle'
```
# 问题分析
由于本人刚刚接触Git，只知道可能是```diff2```这个配置出现了问题。
# 解决办法
使用如下命令将```diff2```换成```diff3```，如果还是不行可以试试```diff```，```diff5```，```diff4```等：
```
77392 (master) new-git-project
$ git config --global merge.conflictstyle diff3
```
这时再次使用```git checkout newbranch```，问题解决。
```
77392 (master) new-git-project
$ git checkout newbranch
Switched to branch 'newbranch'
```
本人对```$ git config --global merge.conflictstyle diff3```这行命令没有理解，如果您有更好的解释欢迎在下方留言。
