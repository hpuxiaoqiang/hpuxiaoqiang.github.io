---
mathjax: true
title: Git命令讲解
date: 2022-8-2
categories:
- Git
tags:
- Git
---

Git 常用命令说明

<!--more-->

# Git

## 初始化

当前文件夹创建一个git仓库

```git
git init
```

添加文档

```git
git add "file name"
```

提交文档

```git
git commit -m "Message"
```

## 时光机穿梭

查看库状态(工作区)

```git
git status
```
对比文件差异(工作区和暂存区)，若暂存区为空，则对比仓库的
查看暂存区和仓库差异
查看工作区和仓库的差异

```git
git diff "file name"
git diff --cached 
git diff HEAD 
```

### 版本回退

查看日志

```git
git log
git log --pretty=online
```

返回到前一个版本

```git
git reset --hard HEAD^
```

返回到指定版本

```git
git reset --hard "CommmitID"
```

查看每一次执行的命令

```git
git reflog
```

### 工作区和暂存区

#### 版本库

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。
<!--->
自建github图床的使用
![stage1](https://raw.githubusercontent.com/hpuxiaoqiang/picbase/main/stage1.png)

![stage2](https://raw.githubusercontent.com/hpuxiaoqiang/picbase/main/stage2.png)
<!--->
<!--->
![图 1](image/stage1.png)
<!--->
![stage1](https://raw.githubusercontent.com/hpuxiaoqiang/picbase/main/stage1.png)
第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。
<!--->
![图 2](image/stage2.png)
<!--->
![stage2](https://raw.githubusercontent.com/hpuxiaoqiang/picbase/main/stage2.png)
一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：
<!--->
![图 3](image/stage3.png)
<!--->
![stage3](https://raw.githubusercontent.com/hpuxiaoqiang/picbase/main/stage3.png)  

### 管理修改

注意如下的操作流程中：

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。因此修改后需要先提交到暂存区中。

### 撤销修改

**情况1**：在工作区做了修改，并未添加到暂存区，想撤销工作区的修改，用 `git restore --worktree "file name"`;

**情况2**：在工作区做了修改，并用`git add`添加到了暂存区，未提交；想撤销，分两步，1.先撤销暂存区的修改，用 `git restore --staged "file name"`, 2.然后参考情况1撤销工作区的修改；

**情况3**：在工作区做了修改，且`git add`、`git commit`添加并提交了内容，想撤销本次提交，直接用 `git reset --hard HEAD^`回退版本，即可保证工作区，暂存区，版本库都是上次的内容。从master同时恢复工作区和暂存区,`git restore --source=HEAD --staged --worktree "file name"`

### 删除文件

如果直接从文件夹里面把文件删除了，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了.现在有两种情况：

- 确实要从版本库中删除该文件，那就用命令`git rm "file name"`删掉，并且`git commit`

- 删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本`git restore -- "file name"`
  
从来没有被添加到版本库就被删除的文件，是无法恢复的！
