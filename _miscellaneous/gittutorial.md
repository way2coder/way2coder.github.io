---
title: "Git Tutorial"
collection: learning
categories:
    - miscellaneous
permalink: /learning/miscellaneous/github/
excerpt: 'Tutorials about basic git command.'
date: 2024-4-24
author_profile: true
toc: true
---

参考链接： 
https://www.runoob.com/git/git-basic-operations.html
https://zhuanlan.zhihu.com/p/35573078

--------------
## Git 基本操作
Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。  
Git 的工作就是创建和保存你项目的快照及与之后的快照进行对比。  
本章将对有关创建与提交你的项目快照的命令作介绍。  
Git 常用的是以下 6 个命令：`git clone`、`git push`、`git add` 、`git commit`、`git checkout`、`git pull`，后面详细介绍。  
<br/><img src='/assets/images/gitbasic.png'>

说明： 
* workspace：工作区
* staging area：暂存区/缓存区
* local repository：版本库或本地仓库
* remote repository：远程仓库

一个简单的操作步骤：
```bash
$ git init    
$ git add .    
$ git commit  
```
* git init - 初始化仓库。
* git add . - 添加文件到暂存区。
* git commit - 将暂存区内容添加到仓库中。

### 创建仓库命令

|命令|说明|  
|----|----|
|git init|初始化仓库|
|git clone|拷贝一份远程仓库，也就是下载一个项目。|

### 提交与修改
Git 的工作就是创建和保存你的项目的快照及与之后的快照进行对比。

|命令|说明|  
|----|----|
|git add	|添加文件到暂存区|
|git status	|查看仓库当前的状态，显示有变更的文件。|
|git diff	|比较文件的不同，即暂存区和工作区的差异。|
|git commit	|提交暂存区到本地仓库。|
|git reset	|回退版本。|
|git rm	|将文件从暂存区和工作区中删除。|
|git mv	|移动或重命名工作区文件。|
|git checkout	|分支切换。|
|git switch （Git 2.23 版本引入）|	更清晰地切换分支。|
|git restore （Git 2.23 版本引入）|	恢复或撤销文件的更改。|

 
### 提交日志

|命令|说明|  
|----|----|
|git log	|查看历史提交记录|
|git blame <file>	|以列表形式查看指定文件的历史修改记录|

### 远程操作
|命令|说明|  
|----|----|
|git remote	|远程仓库操作|
|git fetch	|从远程获取代码库|
|git pull	|下载远程代码并合并|
|git push	|上传远程代码并合并|

### 处理冲突
|命令|说明|  
|----|----|
|git fetch origin; git reset --hard origin/main|将本地分支重置到远程分支的状态| 
|||
-------
## Git 分支管理
几乎每一种版本控制系统都以某种形式支持分支，一个分支代表一条独立的开发线。   
使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作。  
<br/><img src='/assets/images/gitbranch.png'>

列出分支：
```bash
git branch 
```
创建分支命令：
```bash
git branch (branchname)
```
切换分支命令：切换分支以后，会使用将切换的分支的最后快照代替当前目录，因此无需多个目录。
```bash 
git checkout (branchname)
```
创建分支同时并切换：
```bash 
git checkout -b (branchname)
```
合并分支：
```bash
git merge (branchname)
```
删除分支：
```bash
git branch -d (branchname)
```

-----------------------

## Git协同工作流程（以Github为例）
1. 下载 `git clone <url>`
2. 新建并切换到的branch: `git checkout -b (branchname)`
3. 添加代码
4. 保存修改 `git add .; git commit -m "message which tell others what have been changed"`
5. 上传代码 `git push origin (branchname)`邀请团队其他人审核代码
6. 合并分支到master  `git merge (branchname)`
7. 删除你的分支： `git branch -d (branchname)`
8. 忽略系统生成文件，创建一个 .gitignore 文件
