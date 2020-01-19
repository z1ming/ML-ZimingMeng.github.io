---
title: Git commit中的相关引用
date: 2019-11-06 11:00:00
author: Ziming M
top: false
cover: true
summary: 本文介绍了引用之前commit的方式，如HEAD^,HEAD~,HEAD~1等
categories: Git
tags:
  - Git
  - commit
  - 祖先引用
  - HEAD^
---
在Git中，可以使用 SHA、标签、分支和特殊的 ```HEAD ```指针引用 commit。有时候这些并不足够，你可能需要引用相对于另一个 commit 的 commit。例如，有时候你需要告诉 git 调用当前 commit 的前一个 commit，或者是前两个 commit。我们可以使用特殊的“祖先引用”字符来告诉 git 这些相对引用。这些字符为：
- ```^``` – 表示父 commit
- ```~ ```– 表示第一个父 commit

我们可以通过以下方式引用之前的 commit：

- 父 commit – 以下内容表示当前 commit 的父 commit
- HEAD^
- HEAD~
- HEAD~1
- 祖父 commit – 以下内容表示当前 commit 的祖父 commit
- HEAD^^
- HEAD~2
- 曾祖父 commit – 以下内容表示当前 commit 的曾祖父 commit
- HEAD^^^
- HEAD~3
```^ ```和``` ~``` 的区别主要体现在通过合并而创建的 commit 中。合并 commit 具有两个父级。对于合并commit，```^ ```引用用来表示第一个父 commit，而``` ^2 ```表示第二个父 commit。第一个父 commit 是当你运行 git merge 时所处的分支，而第二个父 commit 是被合并的分支。

我们来看一个示例，这样更好理解。这是我的 ```git log ```当前的显示结果：
```
* 9ec05ca (HEAD -> master) Revert "Set page heading to "Quests & Crusades""
* db7e87a Set page heading to "Quests & Crusades"
*   796ddb0 Merge branch 'heading-update'
|\  
| * 4c9749e (heading-update) Set page heading to "Crusade"
* | 0c5975a Set page heading to "Quest"
|/  
*   1a56a81 Merge branch 'sidebar'
|\  
| * f69811c (sidebar) Update sidebar with favorite movie
| * e6c65a6 Add new sidebar content
* | e014d91 (footer) Add links to social media
* | 209752a Improve site heading for SEO
* | 3772ab1 Set background color for page
|/  
* 5bfe5e7 Add starting HTML structure
* 6fa5f34 Add .gitignore file
* a879849 Add header to blog
* 94de470 Initial commit
```
我们来看看如何引用一些之前的 commit。因为 ```HEAD``` 指向 ```9ec05ca ```commit：

- ```HEAD^ ```是```db7e87a```commit
- ```HEAD~1 ```同样是 ```db7e87a``` commit
- ```HEAD^^ ```是 ```796ddb0``` commit
- ```HEAD~2 ```同样是 ```796ddb0``` commit
- ```HEAD^^^ ```是 ```0c5975a``` commit
- ```HEAD~3 ```同样是 ```0c5975a``` commit
- ```HEAD^^^2 ```是 ```4c9749e``` commit（这是曾祖父的 (HEAD^^) 第二个父 commit (^2))
如果你掌握了，不妨回答以下问题：
```HEAD~6```引用的是哪一个 commit？
答案：```209752a ```
